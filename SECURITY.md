# Security notes (public)

This is the public-safe summary. Production secrets are not in this repository.

## What stays private

- API keys and `.env` files
- Owner license-key exports
- Production databases and user/bet history
- Proprietary model weights

## Product controls (high level)

- Activation is username + activation key
- The backend redeems hashed keys and issues a signed session token
- STANDARD licenses expire on a server-side clock after first redemption
- Unlimited license types do not automatically grant owner/admin roles
- Secrets are not shipped in frontend JavaScript
- Critical scoring and licensing logic stays server-side

## Simulated money only

The friends leaderboard and bankroll surfaces use simulated money. The platform does not place real wagers.
