# Depop Tracker + Gmail Auto Fulfillment

This keeps the existing Depop Tracker page and adds a backend for Gmail OAuth and sale-email importing.

## What is included

- Existing tracker UI preserved.
- New **Fulfillment** tab.
- Google OAuth Gmail connection.
- Gmail sale-email scanner.
- Basic sale parser (item, sale price, buyer, date).
- Local fulfillment queue and supplier-product mappings.
- Manual order/tracking status controls.
- Optional server-side fulfillment webhook.

## Important

The browser must NOT contain a Gmail client secret or supplier API secret. The backend keeps those values in environment variables.

Also, direct Depop API access is not assumed. Depop's current Selling API is private and requires approval. If you get access, the email scanner can later be replaced with the official order API/webhook, which is cleaner and more reliable.

Direct AliExpress ordering is also not included as a fake API. If you use a fulfillment provider such as DSers, connect its approved API/integration or a webhook you control. The app's generic webhook endpoint is ready for that step.

## Local setup

1. Install Node.js 20+.
2. Open a terminal in this folder.
3. Run `npm install`.
4. Copy `.env.example` to `.env`.
5. Create a Google Cloud project and enable the Gmail API.
6. Create a Web application OAuth client.
7. Add this redirect URI:
   `http://localhost:3000/oauth2callback`
8. Put the Client ID and Client Secret in `.env`.
9. Run `npm start`.
10. Open `http://localhost:3000`.
11. Log in to your tracker, open **Fulfillment**, and click **Connect Gmail**.
12. Approve Gmail read access.
13. Click **Scan for sales**.

## Production deployment

Use a Node host that keeps a persistent filesystem or replace `data/gmail-token.json` with a real database/secret store. Set `GOOGLE_REDIRECT_URI` to your production callback URL and add that exact URL to the Google OAuth client.

For multiple users, do not keep one token file. Store tokens encrypted per account in a database and tie them to your real authenticated user ID.


## Gmail + AliExpress setup

This project is structured so Gmail is connected through a backend OAuth flow instead of Gmail forwarding.

1. Create a Google Cloud project and enable Gmail API.
2. Create an OAuth Web Application client.
3. Put the client ID/secret in the server environment variables.
4. Set the OAuth redirect URL to `/auth/google/callback` on your deployed site.
5. Start the app and use **Connect Gmail**.
6. Use **Scan Gmail** to pull matching Depop sale messages.
7. Map each tracker item to its AliExpress product URL/SKU.
8. Review orders before sending them to the supplier.

### Important AliExpress limitation

The app does not pretend that a normal AliExpress account can be controlled by arbitrary browser code. Direct automated ordering requires an available/approved integration. DSers supports placing AliExpress orders through its workflow and can sync tracking after the supplier ships. Its current developer program is available for approved custom integrations.

Until an approved ordering API is connected, the app keeps the order in **Ready to order** and provides the supplier information needed to finish the order safely.
