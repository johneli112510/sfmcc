# SMFCC

Choir rehearsal site: upload sheet music (PDF, PNG, JPG, WebP), up to 10 MP3 practice
tracks per piece, and up to 20 YouTube vocal practice guide links per piece (video or
playlist), each tagged Soprano, Alto, Tenor, Bass or Other. Everyone can view and listen;
only someone with the admin password can add, edit or delete.

Files (keep them at the repo root, no folders):
- `worker.js`      the whole app: web page + API
- `wrangler.toml`  Worker name `smfcc` + R2 bucket `smfcc`

## 1. Put it on GitHub
1. Create a new repository (for example `smfcc`).
2. Add file > Upload files, drag in `worker.js`, `wrangler.toml`, `README.md`,
   `.gitignore` and `.dev.vars.example`, then Commit.
3. Check the repo's top level shows `worker.js` and `wrangler.toml` (not inside a folder).

## 2. Create the R2 bucket
Cloudflare dashboard > R2 object storage > Create bucket > name it `smfcc`.

## 3. Deploy on Cloudflare
1. Workers & Pages > Create application > Import a repository, choose your repo.
2. Project/Worker name: `smfcc` (must match `name` in wrangler.toml).
3. Build command: leave empty. Deploy command: `npx wrangler deploy`. Save and deploy.
4. Open the Worker > Settings > Variables and Secrets > Add, type Secret,
   name `ADMIN_PASSWORD`, value = your password. Then Deployments > retry (or push a commit).
5. Visit https://smfcc.<your-subdomain>.workers.dev, click Admin login, add a piece,
   then upload its score and MP3s and paste YouTube links on the piece page.

## Deploy from your computer instead
    npm i -g wrangler
    wrangler login
    wrangler r2 bucket create smfcc
    wrangler deploy
    wrangler secret put ADMIN_PASSWORD

## Notes
- This is a separate site with its own storage: it does not share songs or the password
  with your voice-guides site.
- One file can be up to 90 MB (Cloudflare caps a request at 100 MB on the free plan).
- Use a long password; there is no lockout on repeated attempts.
- Change the subtitle by searching for SITE_SUB inside worker.js.
