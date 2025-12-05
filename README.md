<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>BRICKY — Digital Services Hub | GAS</title>
  <meta name="description" content="Bricky Digital Services — premium websites, branding, social setup and GAS deals." />
  <style>
    /* -------- Style A: Black & Gold (Luxury) -------- */
    :root{
      --bg:#070607;
      --card:#0f0f10;
      --muted:#a8a09a;
      --gold:#ffd24d;
      --accent:#ffdd73;
      --glass: rgba(255,255,255,0.03);
      --radius:14px;
      --maxw:1100px;
      --shadow: 0 10px 30px rgba(0,0,0,0.6);
      font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    }
    html,body{height:100%}
    body{
      margin:0;
      background: linear-gradient(180deg,#050505 0%, #0a0a0a 100%);
      color:#eee;
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
      line-height:1.45;
    }
    .wrap{max-width:var(--maxw);margin:40px auto;padding:28px;}
    header{display:flex;align-items:center;gap:18px;margin-bottom:30px}
    .brand{
      display:flex;align-items:center;gap:12px;text-decoration:none;color:inherit;
    }
    .logo{
      width:56px;height:56px;border-radius:12px;background:linear-gradient(135deg,var(--gold),#ffb84d);box-shadow:0 6px 20px rgba(255,210,77,0.12);display:flex;align-items:center;justify-content:center;font-weight:700;color:#0b0b0b;font-size:20px;
    }
    h1{font-size:28px;margin:0;color:var(--gold);text-shadow:0 2px 8px rgba(255,210,77,0.08)}
    .tag{color:var(--muted);margin-top:6px;font-size:14px}
    .hero{
      display:grid;grid-template-columns:1fr 360px;gap:26px;align-items:center;margin-bottom:28px;
    }
    .card{background:linear-gradient(180deg, rgba(255,255,255,0.02), transparent);border-radius:var(--radius);padding:20px;box-shadow:var(--shadow);border:1px solid rgba(255,255,255,0.03)}
    .hero-left p{color:var(--muted);margin-top:12px}
    .btn{
      display:inline-block;padding:12px 18px;border-radius:12px;background:linear-gradient(90deg,var(--gold),#ffc75b);color:#0b0b0b;font-weight:700;text-decoration:none;box-shadow:0 8px 30px rgba(255,210,77,0.12);
    }
    .subtle{background:transparent;border:1px solid rgba(255,255,255,0.04);color:var(--muted);padding:10px 14px;border-radius:10px;text-decoration:none}
    /* Services grid */
    .grid{display:grid;grid-template-columns:repeat(3,1fr);gap:18px;margin-top:20px}
    .service{padding:18px;border-radius:12px;background:var(--card);border:1px solid rgba(255,255,255,0.02)}
    .service h4{margin:0;color:var(--gold)}
    .service p{color:var(--muted);font-size:14px;margin-top:8px}
    /* Portfolio */
    .portfolio{display:grid;grid-template-columns:repeat(3,1fr);gap:14px;margin-top:20px}
    .port{background:linear-gradient(180deg, #0b0b0b, #0f0f10);padding:16px;border-radius:12px;border:1px solid rgba(255,255,255,0.02)}
    .mock{height:160px;border-radius:10px;background:linear-gradient(120deg,#0f0f10,#141414);display:flex;align-items:center;justify-content:center;color:var(--muted);font-weight:600}
    /* Pricing */
    .pricing{display:grid;grid-template-columns:repeat(3,1fr);gap:14px;margin-top:18px}
    .plan{background:linear-gradient(180deg,#0f0f10, #121212);padding:18px;border-radius:12px;border:1px solid rgba(255,255,255,0.02);text-align:center}
    .plan h3{color:var(--gold);margin:0}
    .plan .price{font-size:22px;color:#fff;margin-top:10px}
    .plan ul{color:var(--muted);text-align:left;margin:12px 0;padding-left:18px}
    /* Testimonials */
    .test-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:14px;margin-top:18px}
    .test{background:var(--glass);padding:14px;border-radius:12px;border:1px solid rgba(255,255,255,0.02)}
    .quote{font-style:italic;color:var(--muted)}
    /* Contact */
    form{display:grid;grid-template-columns:1fr 1fr;gap:10px}
    input,textarea,select{background:#0a0a0a;border:1px solid rgba(255,255,255,0.03);padding:12px;border-radius:10px;color:#eee}
    textarea{grid-column:1 / -1;min-height:120px}
    .muted-sm{color:var(--muted);font-size:13px}
    footer{margin-top:34px;color:var(--muted);text-align:center;padding:18px}
    /* WhatsApp button */
    .wa-btn{position:fixed;right:18px;bottom:18px;background:linear-gradient(90deg,#25d366,#128c7e);color:white;padding:14px;border-radius:999px;display:flex;gap:10px;align-items:center;box-shadow:0 10px 30px rgba(37,211,102,0.16);z-index:999}
    .badge{background:rgba(0,0,0,0.25);padding:6px 10px;border-radius:999px;color:var(--gold);font-weight:700}
    /* Responsive */
    @media (max-width:980px){
      .hero{grid-template-columns:1fr;gap:16px}
      .grid,.portfolio,.pricing{grid-template-columns:repeat(2,1fr)}
      form{grid-template-columns:1fr}
    }
    @media (max-width:560px){
      .grid,.portfolio,.pricing{grid-template-columns:1fr}
      .brand .logo{width:48px;height:48px}
      .wrap{padding:16px;margin:20px auto}
    }
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <a class="brand" href="#">
        <div class="logo">GAS</div>
        <div>
          <div style="display:flex;align-items:center;gap:8px">
            <h1>BRICKY • Digital Services Hub</h1>
            <div class="badge">CEO Infinity</div>
          </div>
          <div class="tag">Premium websites, branding & growth — Black & Gold edition</div>
        </div>
      </a>
    </header>

    <!-- HERO -->
    <section class="hero">
      <div class="hero-left card">
        <h2 style="margin:0">Build your business. Build your legacy.</h2>
        <p class="muted-sm">Fast websites, branding packs, e-commerce setups and WhatsApp funnels — professional results your customers trust. Launch in 24–72 hours.</p>

        <div style="margin-top:18px;display:flex;gap:12px;flex-wrap:wrap">
          <a class="btn" href="#services">Start Your Project</a>
          <a class="subtle" href="#pricing">View Pricing</a>
        </div>

        <div class="grid" style="margin-top:22px">
          <div class="service">
            <h4>24–72h Website Builds</h4>
            <p>Starter, Business or Premium e-commerce. Fast builds, modern UX and mobile-first design.</p>
          </div>
          <div class="service">
            <h4>Branding & Logos</h4>
            <p>Brand packs with logo, color palette and social assets ready to use.</p>
          </div>
          <div class="service">
            <h4>WhatsApp Automation</h4>
            <p>Catalogs, auto replies, and templates — convert leads instantly.</p>
          </div>
        </div>
      </div>

      <aside class="card">
        <h3 style="margin:0;color:var(--gold)">Quick Start Offer</h3>
        <p class="muted-sm" style="margin-top:8px">Get a Starter website + Logo pack for <strong>$19</strong>. Limited spots weekly.</p>

        <div style="margin-top:12px;display:flex;gap:10px">
          <a class="btn" href="#contact">Request Now</a>
          <a class="subtle" href="#portfolio">Portfolio</a>
        </div>

        <div style="margin-top:18px">
          <div style="display:flex;gap:10px;align-items:center">
            <div style="width:46px;height:46px;border-radius:10px;background:linear-gradient(135deg,#ffd24d,#ffb84d);display:flex;align-items:center;justify-content:center;color:#080808;font-weight:800">BR</div>
            <div>
              <div style="font-weight:700">BRICKY • CEO</div>
              <div class="muted-sm">Ready to scale your brand</div>
            </div>
          </div>
        </div>
      </aside>
    </section>

    <!-- SERVICES -->
    <section id="services" style="margin-top:18px">
      <h2 style="color:var(--gold)">Services</h2>
      <div class="grid">
        <div class="service card">
          <h4>Starter Website</h4>
          <p class="muted-sm">One-page responsive site with contact & WhatsApp — perfect for local businesses.</p>
          <div style="margin-top:10px;display:flex;gap:8px">
            <a class="btn" href="#contact">Order $19</a>
          </div>
        </div>
        <div class="service card">
          <h4>Business Website</h4>
          <p class="muted-sm">Multi-page site, portfolio, blog, and email contact — ready for growth.</p>
          <div style="margin-top:10px">
            <a class="btn" href="#contact">Order $29</a>
          </div>
        </div>
        <div class="service card">
          <h4>Premium / E-commerce</h4>
          <p class="muted-sm">Full store, payment setup, shipping options and support.</p>
          <div style="margin-top:10px">
            <a class="btn" href="#contact">Order $99</a>
          </div>
        </div>
      </div>
    </section>

    <!-- PORTFOLIO -->
    <section id="portfolio" style="margin-top:26px">
      <h2 style="color:var(--gold)">Portfolio — sample builds</h2>
      <div class="portfolio">
        <div class="port card">
          <div class="mock">Gustavo Coffee — Demo</div>
          <div style="margin-top:10px">
            <strong style="color:var(--gold)">Gustavo Coffee</strong>
            <div class="muted-sm">Café e-commerce • 24h build</div>
          </div>
        </div>

        <div class="port card">
          <div class="mock">SG Motors — Demo</div>
          <div style="margin-top:10px">
            <strong style="color:var(--gold)">SG Motors</strong>
            <div class="muted-sm">Auto showroom • Catalog + WhatsApp</div>
          </div>
        </div>

        <div class="port card">
          <div class="mock">Health Lab — Demo</div>
          <div style="margin-top:10px">
            <strong style="color:var(--gold)">Health Lab</strong>
            <div class="muted-sm">Clinic • Booking & contact</div>
          </div>
        </div>
      </div>
    </section>

    <!-- PRICING -->
    <section id="pricing" style="margin-top:26px">
      <h2 style="color:var(--gold)">Pricing</h2>
      <div class="pricing">
        <div class="plan card">
          <h3>Starter</h3>
          <div class="price">$19</div>
          <ul>
            <li>1 page responsive</li>
            <li>Contact & WhatsApp</li>
            <li>24–48h delivery</li>
          </ul>
          <a class="btn" href="#contact">Buy</a>
        </div>

        <div class="plan card">
          <h3>Business</h3>
          <div class="price">$29</div>
          <ul>
            <li>3 pages</li>
            <li>Portfolio + SEO basics</li>
            <li>72h delivery</li>
          </ul>
          <a class="btn" href="#contact">Buy</a>
        </div>

        <div class="plan card">
          <h3>Premium</h3>
          <div class="price">$99</div>
          <ul>
            <li>Store or custom</li>
            <li>Payments, shipping</li>
            <li>1 week delivery</li>
          </ul>
          <a class="btn" href="#contact">Buy</a>
        </div>
      </div>
    </section>

    <!-- TESTIMONIALS -->
    <section id="testimonials" style="margin-top:26px">
      <h2 style="color:var(--gold)">Testimonials</h2>
      <div class="test-grid">
        <div class="test card">
          <div style="display:flex;gap:12px;align-items:center">
            <div style="width:56px;height:56px;border-radius:10px;background:linear-gradient(135deg,#ffd24d,#ffb84d);display:flex;align-items:center;justify-content:center;color:#040404;font-weight:800">A</div>
            <div>
              <div style="font-weight:700">Amira — Cafe Owner</div>
              <div class="muted-sm">Nairobi</div>
            </div>
          </div>
          <p class="quote" style="margin-top:10px">"Bricky turned our idea into a beautiful site in 24 hours. Sales increased within days."</p>
        </div>

        <div class="test card">
          <div style="display:flex;gap:12px;align-items:center">
            <div style="width:56px;height:56px;border-radius:10px;background:linear-gradient(135deg,#ffd24d,#ffb84d);display:flex;align-items:center;justify-content:center;color:#040404;font-weight:800">S</div>
            <div>
              <div style="font-weight:700">Sam — Auto Dealer</div>
              <div class="muted-sm">Kisumu</div>
            </div>
          </div>
          <p class="quote" style="margin-top:10px">"Professional and fast. WhatsApp automation saved hours every day."</p>
        </div>
      </div>
    </section>

    <!-- CONTACT -->
    <section id="contact" style="margin-top:26px">
      <h2 style="color:var(--gold)">Contact & Request</h2>
      <p class="muted-sm">Fill this form and we will reach out for a free quote. Or click the WhatsApp button to start chat.</p>

      <!-- Replace action with your Formspree or endpoint -->
      <form class="card" action="https://formspree.io/f/your-id" method="POST">
        <input name="name" placeholder="Full name" required />
        <input name="email" type="email" placeholder="Email" required />
        <input name="phone" placeholder="Phone / WhatsApp" />
        <select name="service">
          <option value="starter">Starter Website — $19</option>
          <option value="business">Business Website — $29</option>
          <option value="premium">Premium / Store — $99</option>
          <option value="branding">Logo & Branding</option>
        </select>
        <textarea name="message" placeholder="Tell us about your project"></textarea>

        <div style="display:flex;gap:12px;align-items:center">
          <button class="btn" type="submit">Send Request</button>
          <a class="subtle" href="mailto:hello@yourdomain.com">Email: hello@yourdomain.com</a>
        </div>

        <div style="margin-top:12px;color:var(--muted);font-size:13px">Payments: PayPal / M-Pesa / Bank transfer. Replace payment links in code where labeled.</div>
      </form>
    </section>

    <footer>
      <div class="muted-sm">© <strong>BRICKY Digital Services</strong> — Built with faith & hustle. • <span style="color:var(--gold)">GAS</span></div>
    </footer>
  </div>

  <!-- WhatsApp floating button (replace number below) -->
  <a id="whatsapp" class="wa-btn" href="#" target="_blank" rel="noopener noreferrer" aria-label="Chat on WhatsApp">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" style="filter:drop-shadow(0 2px 6px rgba(0,0,0,0.2))">
      <path d="M20.5 3.5C18 1 14.8 0 11.4 0 5 0 .3 4.7.3 11.2c0 2 .5 3.9 1.4 5.6L0 24l7.6-2c1.6.9 3.5 1.4 5.7 1.4 6.5 0 11.2-4.7 11.2-11.2 0-3.4-1-6.6-3.5-9.1z" fill="#fff" opacity=".06"/>
      <path d="M17.1 14.3c-.2-.1-1.1-.5-1.4-.6-.4-.1-.6-.2-.9.2-.3.4-1.1.6-1.4.7-.3.1-.5.1-.8-.2-1.6-1.9-2.6-3.7-2.9-4.2-.2-.3 0-.5.1-.6.1-.1.3-.3.6-.5.3-.2.3-.4.5-.7.1-.2 0-.4 0-.6 0-.1-.9-1.9-1.2-2.6-.3-.5-.6-.3-.9-.3-.7 0-1.5.1-2.2 1.1-.7 1-1.1 2.5-1.1 4.1 0 1.6.6 3.1 1.5 4.5 1 1.6 2.6 3.1 4.4 4.1 1.5.8 3.1 1 4.5.8 1.1-.2 2.6-1 2.8-1.9.2-.9.2-1.8.1-1.9-.1-.1-1.1.2-1.3.1z" fill="#25D366"/>
    </svg>
    <span style="font-weight:700">WhatsApp</span>
  </a>

  <script>
    // === Replace these values ===
    const WHATSAPP_NUMBER = "+2547XXXXXXXX"; // change me to your number
    const PAYPAL_LINK = "https://www.paypal.com/paypalme/yourname"; // change to your payment url
    const FORM_ENDPOINT = "https://formspree.io/f/your-id"; // change to your form endpoint
    // =============================

    // wire up contact form action replacement (in case user wants client side change)
    document.querySelectorAll('form').forEach(f=>{
      f.action = FORM_ENDPOINT;
    });

    // WhatsApp button link
    const wa = document.getElementById('whatsapp');
    wa.href = `https://wa.me/${WHATSAPP_NUMBER.replace(/\+/g,'')}`;

    // small UX: scroll to contact when clicking Start / Buy
    document.querySelectorAll('a[href="#contact"]').forEach(a=>{
      a.addEventListener('click', (e)=>{
        e.preventDefault();
        document.getElementById('contact').scrollIntoView({behavior:'smooth'});
      });
    });

    // optional: quick pay modal (example)
    function quickPay(link){
      window.open(link, "_blank");
    }

    // Example: wire buy links to PayPal (if desired)
    document.querySelectorAll('.plan .btn').forEach(b=>{
      b.addEventListener('click', (e)=> {
        e.preventDefault();
        quickPay(PAYPAL_LINK);
      });
    });
  </script>
</body>
</html># FINALE

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
