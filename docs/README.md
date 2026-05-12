# GitHub Pages callback for OpenRouter OAuth

This `docs/` folder is intended to be published with GitHub Pages from the `main` branch.

Required Pages settings for `xy464193/relais`:

1. Go to GitHub repository `Settings` -> `Pages`
2. Set `Source` to `Deploy from a branch`
3. Set `Branch` to `main`
4. Set folder to `/docs`

Published callback URL:

- `https://xy464193.github.io/relais/openrouter/callback/`

This page immediately redirects the browser back into the iOS app using:

- `relais://openrouter/oauth?...`

It is used as the HTTPS `callback_url` for OpenRouter OAuth PKCE.
