# FINALE
/**
 * webhook_server_sample.js
 * Production-ready Express webhook receiver for Travel Approval Workflow
 *
 * Features:
 * - API Key auth (header: x-api-key)
 * - SecurityHash verification (HMAC-SHA256 using SECRET_SIGNING_KEY)
 * - AppSheet API update helper
 * - M-Pesa (Daraja) OAuth + STK Push helper
 * - MPesa callback endpoint (stub/verify)
 * - Logging via winston
 * - Rate limiting
 *
 * Required environment variables (.env):
 * PORT=4000
 * WEBHOOK_API_KEY=replace_with_strong_random_key
 * SECRET_SIGNING_KEY=replace_with_signing_secret_for_hashes
 * APPSHEET_APP_ID=your_appsheet_app_id
 * APPSHEET_ACCESS_KEY=your_appsheet_application_access_key
 * MPESA_CONSUMER_KEY=...
 * MPESA_CONSUMER_SECRET=...
 * MPESA_ENV=sandbox | production
 * MPESA_SHORTCODE=...
 * MPESA_PASSKEY=... (for STK)
 * MPESA_CALLBACK_URL=https://webhook.yourdomain.com/mpesa/callback
 * ADMIN_EMAIL=stunnersonof@gmail.com
 *
 * Install dependencies:
 * npm i express body-parser helmet dotenv node-fetch crypto winston express-rate-limit
 *
 * Run:
 * node webhook_server_sample.js
 *
 * NOTE: This is a template. You MUST secure your server, rotate keys, use HTTPS via nginx/Certbot.
 */

require('dotenv').config();
const express = require('express');
const bodyParser = require('body-parser');
const helmet = require('helmet');
const crypto = require('crypto');
const fetch = require('node-fetch'); // node >= 18 has fetch builtin; using node-fetch for compatibility
const winston = require('winston');
const rateLimit = require('express-rate-limit');

const {
  WEBHOOK_API_KEY,
  SECRET_SIGNING_KEY,
  APPSHEET_APP_ID,
  APPSHEET_ACCESS_KEY,
  MPESA_CONSUMER_KEY,
  MPESA_CONSUMER_SECRET,
  MPESA_ENV,
  MPESA_SHORTCODE,
  MPESA_PASSKEY,
  MPESA_CALLBACK_URL,
  PORT = 4000
} = process.env;

if (!WEBHOOK_API_KEY || !SECRET_SIGNING_KEY) {
  console.error('Missing required env vars: WEBHOOK_API_KEY or SECRET_SIGNING_KEY. Exiting.');
  process.exit(1);
}

/* -------------------------
   Simple logger (winston)
   -------------------------*/
const logger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.printf(info => `[${info.timestamp}] ${info.level.toUpperCase()}: ${info.message}`)
  ),
  transports: [new winston.transports.Console()]
});

/* -------------------------
   Express App + security
   -------------------------*/
const app = express();
app.use(helmet());
app.use(bodyParser.json({ limit: '1mb' }));

// Rate limiter - adjust as necessary
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 200
});
app.use(limiter);

/* -------------------------
   Middleware: API Key auth
   -------------------------*/
function apiKeyAuth(req, res, next) {
  const key = (req.headers['x-api-key'] || '').toString();
  if (!key || key !== WEBHOOK_API_KEY) {
    logger.warn(`Unauthorized request from ${req.ip}`);
    return res.status(401).json({ error: 'Unauthorized' });
  }
  return next();
}

/* -------------------------
   Utility: HMAC SHA256 verify
   - Expected: HMAC_SHA256(SECRET_SIGNING_KEY, canonicalPayload)
   - canonicalPayload: JSON string of payload fields in agreed order (see caller)
   -------------------------*/
function computeHmac(payloadString) {
  return crypto.createHmac('sha256', SECRET_SIGNING_KEY).update(payloadString, 'utf8').digest('hex');
}

/* -------------------------
   AppSheet API helper
   -------------------------*/
async function appsheetEditRow(tableName, rowObj) {
  if (!APPSHEET_APP_ID || !APPSHEET_ACCESS_KEY) {
    throw new Error('AppSheet credentials missing in env');
  }
  const url = `https://api.appsheet.com/api/v2/apps/${APPSHEET_APP_ID}/tables/${encodeURIComponent(tableName)}/Action`;
  const body = {
    Action: 'Edit',
    Properties: { Locale: 'en-US' },
    Rows: [rowObj]
  };
  const resp = await fetch(url, {
    method: 'POST',
    headers: {
      'ApplicationAccessKey': APPSHEET_ACCESS_KEY,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(body)
  });
  const json = await resp.json();
  if (!resp.ok) {
    logger.error('AppSheet API error: ' + JSON.stringify(json));
    throw new Error('AppSheet API failed: ' + (json.error || JSON.stringify(json)));
  }
  return json;
}

/* -------------------------
   M-Pesa (Daraja) helpers
   - getMpesaToken()
   - lipaNaMpesaSTK()
   - handleMpesaCallback()
   -------------------------*/
const MPESA_BASE = (MPESA_ENV === 'production') ? 'https://api.safaricom.co.ke' : 'https://sandbox.safaricom.co.ke';

async function getMpesaToken() {
  if (!MPESA_CONSUMER_KEY || !MPESA_CONSUMER_SECRET) {
    throw new Error('M-Pesa credentials not set');
  }
  const url = `${MPESA_BASE}/oauth/v1/generate?grant_type=client_credentials`;
  const auth = Buffer.from(`${MPESA_CONSUMER_KEY}:${MPESA_CONSUMER_SECRET}`).toString('base64');
  const resp = await fetch(url, {
    method: 'GET',
    headers: {
      Authorization: `Basic ${auth}`
    }
  });
  const data = await resp.json();
  if (!resp.ok) {
    logger.error('M-Pesa token error: ' + JSON.stringify(data));
    throw new Error('M-Pesa token error');
  }
  return data.access_token;
}

// Encoded password for STK: base64(shortcode + passkey + timestamp)
function buildStkPassword(shortcode, passkey, timestamp) {
  return Buffer.from(`${shortcode}${passkey}${timestamp}`).toString('base64');
}

async function lipaNaMpesaSTK(amount, phoneNumber, accountRef, callbackUrl) {
  const accessToken = await getMpesaToken();
  const url = `${MPESA_BASE}/mpesa/stkpush/v1/processrequest`;
  const timestamp = new Date().toISOString().replace(/[-:TZ.]/g, '').slice(0, 14);
  const password = buildStkPassword(MPESA_SHORTCODE, MPESA_PASSKEY, timestamp);
  const body = {
    BusinessShortCode: MPESA_SHORTCODE,
    Password: password,
    Timestamp: timestamp,
    TransactionType: 'CustomerPayBillOnline',
    Amount: amount,
    PartyA: phoneNumber,
    PartyB: MPESA_SHORTCODE,
    PhoneNumber: phoneNumber,
    CallBackURL: callbackUrl || MPESA_CALLBACK_URL,
    AccountReference: accountRef || 'Withdrawal',
    TransactionDesc: 'Withdrawal Payment'
  };
  const resp = await fetch(url, {
    method: 'POST',
    headers: {
      Authorization: `Bearer ${accessToken}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(body)
  });
  const data = await resp.json();
  if (!resp.ok) {
    logger.error('STK push error: ' + JSON.stringify(data));
    throw new Error('STK push failed');
  }
  return data;
}

/* -------------------------
   Endpoint: /webhook/withdrawal
   - expects JSON payload with RequestID, User, TotalCost, Status, SecurityHash, etc.
   - middleware: apiKeyAuth
   - verifies SecurityHash (HMAC)
   - processes request: e.g., validate, create job, call mpesa if instructed, update AppSheet
   -------------------------*/
app.post('/webhook/withdrawal', apiKeyAuth, async (req, res) => {
  try {
    const payload = req.body || {};
    // Basic validation
    const { RequestID, User, TotalCost, Status, SecurityHash, MPesaNumber, Action } = payload;
    if (!RequestID || !User || !TotalCost || !SecurityHash) {
      logger.warn('Bad payload: missing required fields');
      return res.status(400).json({ error: 'Missing required fields' });
    }

    // Canonical payload string - MUST match client-side generation
    // Use consistent ordering of fields — change as needed (communicate to AppSheet generator)
    const canonical = `${RequestID}|${User}|${TotalCost}|${Status || ''}`;
    const expected = computeHmac(canonical);

    if (expected !== SecurityHash) {
      logger.warn(`Invalid SecurityHash for ${RequestID} - expected ${expected}, got ${SecurityHash}`);
      return res.status(403).json({ error: 'Invalid SecurityHash' });
    }

    logger.info(`Webhook received valid payload for ${RequestID} (action: ${Action || 'none'})`);

    // Example: if action instructs to pay, trigger M-Pesa STK
    if (Action === 'PAY' && MPesaNumber) {
      logger.info(`Triggering STK push for ${RequestID} -> ${MPesaNumber} amount ${TotalCost}`);
      try {
        const stkResp = await lipaNaMpesaSTK(TotalCost, MPesaNumber, RequestID);
        // You may want to store stkResp.TransactionId or CheckoutRequestID in DB for correlation
        logger.info(`STK push response: ${JSON.stringify(stkResp)}`);

        // Optionally, update AppSheet record to note STK initiated
        await appsheetEditRow('Withdrawals', {
          ID: RequestID,
          Status: 'Payment Initiated',
          AuditNotes: `STK initiated: ${stkResp.CheckoutRequestID || stkResp.ResponseDescription || JSON.stringify(stkResp)}`
        });
        return res.json({ status: 'stk_initiated', detail: stkResp });
      } catch (err) {
        logger.error('Error during STK push: ' + err.message);
        await appsheetEditRow('Withdrawals', {
          ID: RequestID,
          AuditNotes: `STK error: ${err.message}`
        }).catch(e => logger.warn('AppSheet update failed: ' + e.message));
        return res.status(500).json({ error: 'STK push failed', detail: err.message });
      }
    }

    // If not a payment action — maybe it's a record creation or status sync
    if (Action === 'CREATE' || Action === 'UPDATE') {
      // Translate payload to AppSheet edit or add as needed.
      const row = {
        ID: RequestID,
        Status: Status || 'Pending',
        AuditNotes: `Webhook received from ${User} at ${new Date().toISOString()}`
      };
      await appsheetEditRow('Withdrawals', row);
      return res.json({ status: 'ok', message: 'AppSheet updated' });
    }

    // Default: just accept and log
    return res.json({ status: 'received' });
  } catch (err) {
    logger.error('Webhook error: ' + err.stack);
    return res.status(500).json({ error: 'server_error', detail: err.message });
  }
});

/* -------------------------
   Endpoint: /mpesa/callback
   - receives Safaricom callback after STK push or B2C
   - verify structure, log, update AppSheet accordingly
   -------------------------*/
app.post('/mpesa/callback', async (req, res) => {
  try {
    const body = req.body || {};
    logger.info('MPesa callback received: ' + JSON.stringify(body));

    // Example of handling typical STK callback:
    // body.Body.stkCallback.ResultCode, Body.stkCallback.CheckoutRequestID, etc.
    // Validate and map to RequestID (we suggested using AccountReference=RequestID)

    // Extract checkout request and result
    const stk = body?.Body?.stkCallback;
    if (stk) {
      const checkoutRequestID = stk.CheckoutRequestID;
      const resultCode = stk.ResultCode;
      // Search your storage for AccountReference mapping (if you saved it on STK response)
      // For this sample, we assume AccountReference == RequestID, which your MPesa provider must include in callback's metadata.
      // Parse metadata (if present)
      const callbackMetadata = stk?.CallbackMetadata;
      let requestId = null;
      if (callbackMetadata && callbackMetadata.Item) {
        // Try to find account reference in metadata items
        const acct = callbackMetadata.Item.find(i => i.Name === 'AccountReference' || i.Name === 'Account No' || i.Name === 'BillRefNumber');
        if (acct) requestId = acct.Value;
      }
      // Fallback: you might store mapping from CheckoutRequestID -> RequestID in DB earlier.
      // Update AppSheet accordingly
      const status = (resultCode === 0) ? 'Paid' : 'Payment Failed';
      const paidDate = new Date().toISOString();
      const rowUpdate = requestId ? { ID: requestId, Status: status, PaidDate: paidDate, AuditNotes: `M-Pesa callback: ${JSON.stringify(stk)}` } : null;

      if (rowUpdate) {
        try {
          await appsheetEditRow('Withdrawals', rowUpdate);
          logger.info('AppSheet updated after MPesa callback for ' + requestId);
        } catch (e) {
          logger.error('Failed to update AppSheet after MPesa callback: ' + e.message);
        }
      } else {
        logger.warn('Could not determine RequestID from MPesa callback. Save record for manual resolution.');
      }
      // respond 200 to Safaricom quickly
      return res.json({ ResultCode: 0, ResultDesc: 'Accepted' });
    }

    // If unknown format
    return res.status(400).json({ error: 'unknown mpesa callback format' });
  } catch (err) {
    logger.error('MPesa callback handler error: ' + err.stack);
    return res.status(500).json({ error: 'server_error' });
  }
});

/* -------------------------
   Health check & root
   -------------------------*/
app.get('/health', (req, res) => res.json({ status: 'ok', timestamp: new Date().toISOString() }));
app.get('/', (req, res) => res.send('Travel Approval Webhook Service'));

/* -------------------------
   Graceful shutdown
   -------------------------*/
const server = app.listen(PORT, () => {
  logger.info(`Webhook server listening on port ${PORT}`);
});

process.on('SIGINT', () => {
  logger.info('SIGINT received: shutting down');
  server.close(() => process.exit(0));
});
process.on('SIGTERM', () => {
  logger.info('SIGTERM received: shutting down');
  server.close(() => process.exit(0));
});

/* -------------------------
   Unit test stubs (Jest) - save as webhook_server.test.js in __tests__ folder
   (Not run here; provided as helper)
   -------------------------*/

/*
Example Jest test file (not executed here):

// __tests__/webhook_server.test.js
const request = require('supertest');
const app = require('../webhook_server_sample'); // export app if you refactor

describe('Webhook endpoints', () => {
  test('health check', async () => {
    const res = await request(app).get('/health');
    expect(res.statusCode).toBe(200);
    expect(res.body.status).toBe('ok');
  });

  test('unauthorized webhook', async () => {
    const res = await request(app)
      .post('/webhook/withdrawal')
      .send({ RequestID: 'TST1', User: 'a@b.com', TotalCost: 100, Status: 'Submitted', SecurityHash: 'x' });
    expect(res.statusCode).toBe(401);
  });
});

To run tests:
npm i --save-dev jest supertest
Add to package.json: "test": "jest"
*/
FINALE is a lightweight WebApp to view [LabVIEW](https://www.ni.com/en-in/shop/labview.html) code. FINALE stands for FINALE Is Not A LabVIEW Editor.

This solves many use cases like:
  - Code sharing: Sharing LabVIEW code with a person who does not have LabVIEW installed.
  - Viewing LabVIEW code without launching LabVIEW.
  - Viewing LabVIEW code saved in incompatible version.
  - Viewing LabVIEW code that is being used with TestStand.

# Contents

- Features
- Setting up FINALE for your code

# Features

  - A left pane to display project hierarchy.
  - Search functionality to find things of interest quickly.
  - Support for viewing the following file types:
    - VI
    - CTL
    - LVClass
    - LLB
    - LVProj
    - Polymorphic VI
  - Support for viewing Multi Frame Structures like:
    - Case Structures
    - Event Structures
    - Diagram Disable Structures
    - Stacked Sequence Structures
  - Navigation to SubVIs and Dynamic Dispatch SubVIs.

Note: FINALE is not supported by National Instruments and this is mostly internal tooling that we are exposing. This is a work in progress, and is not yet feature complete.

# Setting up FINALE for your code

FINALE has two parts, the HTML Generator and the WebApp. The HTML Generator converts LabVIEW code to the FINALE format (a composition of JSON documents, images, etc., which is understood by the WebApp). These are input to the WebApp which opens a web-based viewer for the files converted. 
## Prerequisites: 
- LabVIEW: Required only for converting the files
- Browser: Google Chrome/Firefox (Does not have complete support in Edge)
- Python 3+ : For CLI tool
- [npm](https://www.npmjs.com/get-npm)
- [npm http-server](https://www.npmjs.com/package/http-server) or IIS
>Note: If there are errors with the npm http-server, try installing at this version:
>
>`npm install –g http-server@0.9.0`

The WebApp is developed using NPM. Run the following commands to produce binaries under a "build" directory.
```sh
git clone https://github.com/ni/finale && cd finale
npm install
npm run build-webapp
```

## Running FINALE:
Follow these instructions to run FINALE:
- Once you have the repository built and set up according to the above commands, proceed to the next step.

- ### Converting LabVIEW code to FINALE format
  This can be done in two ways:

    - #### Running the Converter VI: 
      - Navigate to "build/HTMLGenerator/".

      - Open Main.vi and enter values for the following:
        - Source directory/files: Path to the source LabVIEW code file(s) or folder.
          >Note: When converting a folder containing LV projects, the converted folder in the WebApp does not have information about the projects. This is expected behaviour.
        - Destination Directory: Path to the destination directory. To view the files using the WebApp, make sure your destination is set to "<Path/to/FINALE/repo>/build/src".
        - Run Main.vi and click "Convert".
      ![Main.vi](./docs/Main.vi.png)
      ##### Converting multiple projects:
        - Open GenerateUI-Advanced.vi at "build/HTMLGenerator/" and enter values for the following:
           - Top level output path: <Path/to/FINALE/repo>/build/src
           - Files to Preload: Array of files you want to preload.
           - File(s)/Folder to convert: Path to the source LabVIEW code file(s) or  folders.
           - Destination Folder (relative to output path): This is an optional field to specify an output path for the converted files. This must be relative to the Top level output path.
           > Note: "Files to Preload" and "Destination Folder (relative to output path)" are optional. If left empty, it is equivalent to running Main.vi. This VI can be used to convert single projects as well.

    - #### Using CLI:
      Prerequisite: PathLib module (https://pypi.org/project/pathlib/)

      - On cmd or PowerShell, navigate to "<Path/to/FINALE/repo>/build/HTMLGenerator".
      - Run command:
      > `python converter.py <path to JSON file>"`
      - The JSON file mentioned above should be of the structure:
        ```json
          {
            "topPath": "Absolute path where you want the FINALE format to be stored",
            "configurations": [
              {
                "inputPath": "Absolute path of the source files/directory that needs to be converted",
                "outputPath": "Relative path to `topPath` so that the output of the converter can be redirected to this path instead of the `topPath`",
                "preloadFiles": "File/Project that needs to be preloaded to load up the actual files that need to be converted"
              },
              {
                "inputPath": "...",
                "outputPath": "...",
                "preloadFiles": "..."
              }
            ]
          }
        ```
      >  - Add more elements to the "configurations" array to convert multiple projects.
      >  - "topPath" and "inputPath" (in "configurations" array) are required keys, the other keys "outputPath" and "preloadFiles" are optional.


- ### Using the WebApp:
  The WebApp reads the FINALE format stored in the "build" directory. To launch the WebApp:
  - On cmd or PowerShell, navigate to the "build" directory in the repo and start the npm http-server:
  > `http-server [-p PORTNUMBER]`
  - Alternatively, IIS can also be used to host the server.
  - The above command launches the server at the displayed address where the FINALE format files can be viewed.
  - FINALE should now be ready to use!
  - #### Adding more converted files
    If more converted files/folders need to be added at this point,
    - Convert the new projects to a different location using Main.vi. (Main.vi first deletes the destination directory.)
    - Copy these FINALE format files "<Path/to/FINALE/repo>/build/src". Make sure to rename the new file.json so that the existing file.json does not get replaced.
    - Create a config.txt file that lists these .json files. The paths should be relative to the root of the server, that is, the "build" directory.
      For example, if their are 3 FINALE format files (foo + foo.json), (bar + bar.json) and (baz + baz.json), and "foo" and "bar" need to be viewed, config.txt should list these .json files like so:
    ```sh
      /src/foo.json
      /src/bar.json
    ```

# Contributing to the project

Contributions to FINALE are welcome from all!

For more details, see [Contributing.md](./Contributing.md)
