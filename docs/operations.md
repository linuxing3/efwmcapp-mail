# Cloud Mail operations

## Public routing

| Hostname | Cloudflare Worker |
| --- | --- |
| `mail.efwmcapp.com` | `efwmcapp-mail` |
| `efwmcapp.com` | `efwmcapp-lp` |
| `book.efwmcapp.com` | `efwmcapp-lp` |
| `cozy.efwmcapp.com` | `efwmcapp-lp` |

The deploy workflow uses repository variable `CUSTOM_DOMAIN=mail.efwmcapp.com`.

## Incoming mail

Cloudflare Email Routing owns the MX records for `efwmcapp.com`. Its enabled
catch-all rule sends every `@efwmcapp.com` recipient to `efwmcapp-mail`.
This intentionally replaces Gmail forwarding. Test inbound delivery from a
separate mailbox and read it at `https://mail.efwmcapp.com`.

## Outgoing mail

Cloud Mail uses Resend for messages sent to external recipients. Verify
`efwmcapp.com` in Resend, keep its DNS records current, and add the restricted
Resend sending API key through **System Settings → Resend Token** after logging
in as an administrator.

## Administrator recovery

The administrator email is `overlabor77@efwmcapp.com`. Its password is stored
only in Bitwarden under **Cloud Mail administrator — efwmcapp.com**.

For an emergency reset, generate a new password locally, save it to Bitwarden,
set the short-lived `CLOUD_MAIL_ADMIN_RESET_PASSWORD` repository secret, run
`Reset Cloud Mail administrator password`, and then delete that temporary
secret. The workflow hashes the password in the runner before updating D1; do
not put passwords in repository variables, source files, issues, or logs.

## GitHub secrets

- `CLOUDFLARE_API_TOKEN`
- `CLOUDFLARE_ACCOUNT_ID`
- `JWT_SECRET`

The Cloudflare token needs Workers, D1, KV, DNS/Zone Settings, and Email
Routing permissions for the managed workflows.
