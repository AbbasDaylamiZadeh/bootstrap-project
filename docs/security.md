# Security

## Secrets

Never commit secrets.

Never expose private credentials to Client Components.

Never place secrets in public environment variables.

## Authentication

Keep authentication decisions on trusted server boundaries.

Do not trust client state as proof of authorization.

## Authorization

The backend must enforce authorization.

Frontend authorization is a UX concern, not a security boundary.

## Input Validation

Validate untrusted input.

This includes:
- URL parameters
- form input
- API responses when trust assumptions require it
- uploaded files
- user-generated content

## XSS

Do not use raw HTML rendering unless absolutely necessary.

When raw HTML is required, sanitize it using the project's approved mechanism.

## URLs

Treat external URLs and redirect targets as untrusted input.

Avoid open redirect vulnerabilities.

## Logging

Never log:
- passwords
- access tokens
- refresh tokens
- sensitive personal information
- authorization headers

## Dependencies

Keep dependencies updated according to the team's security policy.

Review advisories before upgrading critical infrastructure.

## Client Data

Only send data to the browser that the client actually needs.

Server-side secrets and privileged data must stay server-side.
