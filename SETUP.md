# BUILD IT — Complete Setup Guide
# Read this once. Follow in order. Takes about 20 minutes.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## HOW THE PROTECTION WORKS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

The flow is simple:

1. Buyer purchases on Payhip
2. Payhip redirects them to your site with a secret token in the URL:
   https://yoursite.netlify.app/access?token=YOUR_SECRET_TOKEN
3. Netlify checks: is this the correct token?
4. YES → sets a secure cookie in their browser → sends them to the guides
5. NO → sends them back to the purchase page
6. Every time they visit /guide/* → Netlify checks for the cookie
7. Cookie lasts 1 year — they can close and reopen anytime

Nobody can access the guides without going through Payhip first.
The guide URLs (/guide/ and /guide/training.html) are not linked
anywhere publicly, so they cannot be found by search engines or guessing.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## YOUR FOLDER STRUCTURE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

built-site/
├── index.html                          ← Your landing page (public)
├── netlify.toml                        ← Netlify configuration
├── SETUP.md                            ← This file
├── guide/
│   ├── index.html                      ← BUILD IT Interactive Guide (protected)
│   └── training.html                   ← BUILD IT Training Guide (protected)
└── netlify/
    └── edge-functions/
        ├── access.js                   ← Token validator
        └── protect.js                  ← Cookie checker (runs on every /guide/* request)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## YOUR ACCESS TOKEN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This is your secret token. KEEP IT PRIVATE.
Never share it publicly. Never put it in your code.

  YOUR_TOKEN_HERE

Save this somewhere safe (Notes app, password manager).
You will need it in Step 3 below.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## STEP 1 — DEPLOY TO NETLIFY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. Go to app.netlify.com
2. Click "Add new site" → "Deploy manually"
3. Drag and drop the entire "built-site" FOLDER into the upload area
   (The whole folder — not individual files)
4. Wait for deploy to complete
5. Copy your site URL (e.g. random-name-123.netlify.app)
6. Optional: go to Site settings → Domain → change to a custom name

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## STEP 2 — SET THE ENVIRONMENT VARIABLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This is where your secret token lives. Without this step, the
access gate will not work.

1. In Netlify, go to your site
2. Click "Site configuration" → "Environment variables"
3. Click "Add a variable"
4. Key:   BUILT_ACCESS_TOKEN
5. Value: YOUR_TOKEN_HERE
6. Click "Save"
7. Go to "Deploys" and click "Trigger deploy" → "Deploy site"
   (Required after adding env variables)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## STEP 3 — UPDATE YOUR LANDING PAGE LINKS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Open index.html and replace all instances of these placeholders:

  REPLACE_GUIDE_LINK      → your Payhip Guide Only listing URL
  REPLACE_COMMUNITY_LINK  → your Payhip Guide + Community listing URL
  YOUR_FORM_ID            → your Formspree form IDs (two forms)
  YOUR_WHATSAPP_NUMBER    → your WhatsApp number (no + or spaces)
  DATE                    → your launch deadline date

After editing, redeploy: drag the folder into Netlify again.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## STEP 4 — CONFIGURE PAYHIP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

For BOTH Payhip product listings (Guide Only + Guide + Community):

1. Go to your Payhip product
2. Click "Edit"
3. Find "Redirect URL" or "Thank you page URL"
4. Paste this EXACT URL (replace yoursite with your actual Netlify URL):

   https://yoursite.netlify.app/access?token=YOUR_TOKEN_HERE

5. Save

Now when anyone buys, Payhip sends them directly to that URL.
The edge function validates, sets the cookie, redirects to the guides.
No file to download. No confusion. Works on every device.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## STEP 5 — UPDATE YOUR INSTAGRAM BIO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Your Instagram bio link: https://yoursite.netlify.app

That's it. The landing page is public. The guides are protected.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## TESTING (DO THIS BEFORE GOING LIVE)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Test 1 — Blocked access:
  Visit: https://yoursite.netlify.app/guide/
  Expected: Redirected to /#pricing (no cookie = no access) ✓

Test 2 — Valid token:
  Visit: https://yoursite.netlify.app/access?token=YOUR_TOKEN
  Expected: Redirected to /guide/ with guides working ✓

Test 3 — Invalid token:
  Visit: https://yoursite.netlify.app/access?token=wrongtoken
  Expected: Redirected to /#pricing ✓

Test 4 — Cookie persistence:
  Close browser. Reopen. Visit /guide/
  Expected: Guides still accessible (cookie persists) ✓

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## IF YOU WANT TO ROTATE THE TOKEN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

If the token gets shared or compromised:
1. Generate a new token (any random long string)
2. Update BUILT_ACCESS_TOKEN in Netlify environment variables
3. Update the redirect URL in Payhip with the new token
4. Redeploy Netlify
5. Existing buyers with valid cookies are NOT affected
   (they already have their cookie and don't need the token URL again)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## WHAT THIS PROTECTS AGAINST
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✓ Direct URL access without purchasing (/guide/ is blocked)
✓ Search engines indexing your guide pages (X-Robots-Tag header)
✓ Iframe embedding on other sites (X-Frame-Options header)
✓ Guide pages being cached by browsers (Cache-Control header)
✓ Token guessing (64-character cryptographically random token)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## WHAT THIS DOES NOT PROTECT AGAINST
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✗ A buyer sharing their access URL (token URL) with others
  → Rotate token if this happens
✗ A buyer saving the HTML files locally after first access
  → Acceptable risk for digital products at this stage
✗ Screenshot or recording of content
  → Unavoidable at any level

This is the right protection level for a beginner creator business.
Enterprise-level DRM costs thousands per month and is overkill here.
