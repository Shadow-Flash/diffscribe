# Example: Diffscribe Output

**Branch:** `feat/123-add-oauth-google-login`
**Base:** `main`
**Template:** `.github/pull_request_template.md`

---

## ✅ Diffscribe Output — Ready to Paste

---

## Summary

This PR adds Google OAuth 2.0 login support to the authentication system, allowing users to sign in using their Google account as an alternative to email/password. It integrates the `passport-google-oauth20` strategy and adds the necessary callback routes and session handling.

## What Changed

**Added**
- `src/auth/google.strategy.ts` — Passport Google OAuth strategy configuration
- `src/auth/oauth.controller.ts` — OAuth callback and redirect route handlers
- `tests/auth/google.strategy.spec.ts` — Unit tests for the OAuth strategy

**Modified**
- `src/auth/auth.module.ts` — Registered Google strategy in the auth module
- `src/config/env.config.ts` — Added `GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET` env vars
- `README.md` — Added OAuth setup instructions

## Motivation & Context

Users were dropping off during signup due to the friction of email/password registration. Adding Google OAuth reduces signup time to a single click and improves conversion. Closes #123.

## How Has This Been Tested?

- [x] Unit tests
- [ ] Integration tests
- [x] Manual testing

Manually tested the full OAuth flow in local environment — Google consent screen, callback redirect, session creation, and user profile persistence. Unit tests cover the strategy configuration and token validation.

## Screenshots (if applicable)

<!-- Add screenshots or screen recordings here -->

## Breaking Changes

No breaking changes. Existing email/password login is unaffected.

- [ ] This PR introduces breaking changes

## Related Issues

Closes #123

## Checklist

- [x] My code follows the project style guidelines
- [x] I have performed a self-review of my code
- [x] I have commented my code where necessary
- [x] I have added tests that prove my fix/feature works
- [x] New and existing tests pass locally
- [ ] I have updated documentation if needed

---

**Key files to review:** `src/auth/google.strategy.ts`, `src/auth/oauth.controller.ts`
