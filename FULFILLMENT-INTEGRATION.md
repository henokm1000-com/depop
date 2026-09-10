# Auto Fulfillment

## Intended flow

Depop sale email
→ Gmail API
→ parser
→ tracker sale
→ product mapping
→ fulfillment order
→ AliExpress/DSers
→ tracking
→ tracker status

## Safety

Do not put Google OAuth client secrets or supplier API secrets in `index.html`.
They belong in server environment variables.

## Current state

The project includes the fulfillment order API scaffold and the frontend project from the uploaded tracker. The supplier step stays in a reviewable state until an approved AliExpress/DSers ordering connection is configured.
