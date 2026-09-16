# The fixed address for the demo (JOB-GUI10, Route B)

This folder is one static page. Its address is the FIXED address that goes on the thank-you slide's QR code.
The page does nothing but forward the phone to the day's Cloudflare quick-tunnel address, which changes at every
start of `start_demo.bat`. The phone sees "Opening the demo" for a fraction of a second and lands on the app.

## Publish it on GitHub Pages (five steps, done once, needs a GitHub account)

1. On github.com, create a new PUBLIC repository named `hybrid-demo` (any name works; the name is the last part of
   the address). Do not add a README or licence there.
2. On this laptop, in a terminal in `D:\GUI10\redirect`, connect the folder to it and push (the repo is already
   initialised and committed here):

       git remote add origin https://github.com/<your-github-username>/hybrid-demo.git
       git branch -M main
       git push -u origin main

   Git will open a browser window to sign in the first time (Git Credential Manager); after that pushes are silent.
3. On github.com, open the repository, then Settings, then Pages. Under "Build and deployment" choose
   Source: "Deploy from a branch", Branch: `main`, folder `/ (root)`. Save.
4. Wait about a minute, reload the Pages settings page: it shows "Your site is live at
   https://<your-github-username>.github.io/hybrid-demo/". That is the fixed address.
5. Put that address into `D:\GUI_APP\config\demo.json` as `"public_url"` and run
   `D:\GUI_APP\tools\make_qr_public.bat https://<your-github-username>.github.io/hybrid-demo/` to make the two
   QR images (`qr_public.png`, `qr_public_slide.png`) for the slide.

## Updating the one line on the day

`start_demo.bat` starts the tunnel, reads its new address, rewrites the two redirect lines in this `index.html`
(lines 10 and 11, the `<meta http-equiv="refresh">` and the `location.replace(...)`; both carry the same address),
commits, and:

- if this folder has a git remote named `origin`, it pushes. GitHub Pages redeploys in about 10 to 60 s.
  `start_demo` waits up to 120 s for the fixed address to forward to the app before showing the slide code.
- if there is no remote, it prints `publish this file: D:\GUI10\redirect\index.html`. Then either push by hand
  (`git push`) or paste the file's content into the repository on github.com (edit index.html, commit).

To update by hand at any time: change the address in both lines, keep everything else, commit and push.

## What the page contains

- `<meta http-equiv="refresh" content="0; url=...">`: an instant redirect that works with JavaScript off.
- `location.replace(...)`: the same redirect in JavaScript, so the fixed address does not stay in the phone's history.
- `noindex`, no visible content beyond "Opening the demo", no tracking, no cookies.
- `.nojekyll` stops GitHub Pages from running Jekyll on the folder.
