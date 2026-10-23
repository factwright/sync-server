# Factwright’s sync server, ready to deploy

Factwright’s sync server, built and ready to run in your own Cloudflare account, for free. Factwright, Save to Factwright and the web view on your phone can all sync through it, end-to-end encrypted: the server only ever holds locked data.

This is the server from Factwright 0.12.1, made from [apps/server at 7a42c0d](https://github.com/factwright/factwright/tree/7a42c0d1de6d4ffa87de1e70c38a7e954f3bcb41/apps/server) with nothing changed. Its source is there, under the AGPL.

[![Deploy to Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/factwright/sync-server)

## Before you deploy

You need a free Cloudflare account and a GitHub account.

1. **Your address.** In Cloudflare, open Workers & Pages and find your workers.dev subdomain. If it asks, pick one. Your server will be at `https://factwright-sync.<subdomain>.workers.dev`.
2. **A GitHub sign-in app.** At [github.com/settings/applications/new](https://github.com/settings/applications/new), make an OAuth app with:
   - Homepage URL: `https://factwright-sync.<subdomain>.workers.dev`
   - Authorization callback URL: `https://factwright-sync.<subdomain>.workers.dev/api/auth/callback/github`

   Then generate a client secret.
3. **Deploy.** Click Deploy to Cloudflare above, and keep the server’s name, `factwright-sync`: your address and your GitHub app use it. It makes the database and the file storage, which can have any names. When it asks for:
   - `BETTER_AUTH_SECRET`: Signs sign-ins. Any long random text, such as what `openssl rand -base64 32` prints.
   - `GITHUB_CLIENT_ID`: Your GitHub OAuth app’s client ID, for signing in with GitHub.
   - `GITHUB_CLIENT_SECRET`: Your GitHub OAuth app’s client secret.
   - `ALLOWED_GITHUB_USERS`: Your GitHub user name, such as `octocat`: only the accounts named here can sign in. To let in someone else, add their user name after a comma. Left empty, anyone with GitHub can.

It runs on Cloudflare’s free plan. Cloudflare may ask for a payment method before it turns on file storage (R2), even within the free allowance.

## Using it

Open your server’s address to sign in. To sync Factwright through it, follow [Running your own](https://github.com/factwright/factwright/blob/main/apps/server/README.md#running-your-own).

## Deploying from a terminal

```bash
npm install
npx wrangler login
npx wrangler deploy   # the first time, to make the database and the file storage
npx wrangler secret put BETTER_AUTH_SECRET
npx wrangler secret put GITHUB_CLIENT_ID
npx wrangler secret put GITHUB_CLIENT_SECRET
npx wrangler secret put ALLOWED_GITHUB_USERS
npm run deploy        # sets up the database's tables, then deploys
```

Each Factwright release puts its server here.
