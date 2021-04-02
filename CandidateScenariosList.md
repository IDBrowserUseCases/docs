---
title: "[]{#_rsg8k1ef0cpn .anchor}Atomic scenarios for identity in the
  browser flows"
---

This document captures the list of scenarios we believe should be
described as part of this effort. Please refer to the corresponding
issue entries for assignment and reference to individual scenario
documents.

# Sign in

## Simple redirect sign in with 1 RP - OIDC implicit+form post

Web application RP1 offers sign in/sign up functionality for users of
identity provider IP1, using OpenID Connect implicit flow and form_post.
Ignoring how IP1 auths the user, apart from the fact that successful
auth results in a cookie in IP1 domain. Notable: the IDToken might not
contain user profile info, accessible via userinfo call from the server
(no user agent access) in case of hybrid variant.

[[\[Sign In\] Simple redirect sign in with 1 RP using OIDC implicit+form
post · Issue \#4 · IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/4)

## Simple redirect sign in with 1 RP - OIDC code flow

Web application RP1 offers sign in/sign up functionality for users of
identity provider IP1, using OpenID Connect code flow. Ignoring how IP1
auths the user, apart from the fact that successful auth results in a
cookie in IP1 domain. Notable: IDtoken is obtained server side, no user
agent access.

[[\[Sign In\] Simple redirect sign in with 1 RP with OIDC code flow ·
Issue \#5 · IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/5)

## Simple redirect sign in with 1 RP - SAML Web SSO (redirect, post)

Web application RP1 offers sign in/sign up functionality for users of
identity provider IP1, using SAML. Ignoring how IP1 auths the user,
apart from the fact that successful auth results in a cookie in IP1
domain. Notable: the SAML assertion might be fully encrypted, hence
opaque to the user agent..

[[\[Sign In\] Simple redirect sign in with 1 RP - SAML Web SSO
(redirect, post) · Issue \#6 · IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/6)

## Simple redirect sign in with 1 RP - SAML Web SSO (artifact)

\[to the SAML people: do we need to document this?\]

[[\[Sign In\] Simple redirect sign in with 1 RP - SAML Web SSO
(artifact) · Issue \#7 · IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/7)

## Simple redirect sign in with 1 RP - WS-Federation

\[to the MS people: do you still have many apps in the wild using
WS-Fed?\]

[[\[Sign In\] Simple redirect sign in with 1 RP - WS-Federation · Issue
\#8 · IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/8)

## IDP initiated sign in

The details of the flow vary depending on protocol (OIDC has an extra
full redirect round trip while SAML is effectively just one x-site
navigation). But typically a user with a currently authenticated session
(which likely maybe was established via SSO itself) is presented with a
portal-like page with links to applications they can access. Navigating
to one of those applications kicks off the IDP initiated SSO flow that
ultimately delivers a SSO token to the RP application and the user is,
from their perspective anyway, signed in seamlessly. I don't think
IDP-init uses browser features any more or differently than the SP-init
variants (1st party samesite none/lax cookies and various things that
look like link decoration). But it doesn't fit the WebID model (last
I've seen of it anyway) where the UX assumes things start on an RP site.

## [\[Sign In\] IDP-initiated sign in · Issue \#9 · IDBrowserUseCases/docs (github.com)]{.ul}

## Poor man's OAuth2 sign in with 1 RP - "OAuth2 code flow abuse"

Web application RP1 offers sign in/sign up functionality for users of
identity provider IP1, abusing OAuth2 (eg conducting an OAuth2
authorization code flow, attempting an API call with the resulting
access token and considering the user signed in eg creating a session
cookie if the call succeeds).. Ignoring how IP1 auths the user, apart
from the fact that successful auth results in a cookie in IP1 domain.
Notable: the user agent doesn;t see any user info, all exchanges occur
server side.

[[\[Sign In\] Quick and Dirty OAuth 2.0 Sign In with 1 RP - "OAuth2 code
flow abuse" · Issue \#10 · IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/10)

## Redirect based SSO - User already signed in RP1, need to SSO in RP2 - both RP1 and RP2 trust IP1

Web application RP1 and RP2 offer sign in/sign up functionality for
users of identity provider IP1, using \[any OpenID Connect flow\| any
SAML flow \| any WS-Fed flow \| any proprietary cookie based auth
scheme\] . The user is already signing in RP1. The user navigates to
RP2, and expects to obtain an authenticated session without any
interactive prompt.\
User agent access to user info depends on the mechanics of the protocol
of choice.

[[\[Sign In\] Redirect based SSO - User already signed in RP1, need to
SSO in RP2 - both RP1 and RP2 trust IDP1 · Issue \#11 ·
IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/11)

# Sign out/session management

## OpenID Connect RP initiated logout

[[\[Sign Out / SM\] OpenID Connect RP-initiated logout · Issue \#12 ·
IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/12)

## OpenID Connect Frontchannel logout \*\*

[[\[Sign Out / SM\] OpenID Connect Front-Channel Logout · Issue \#13 ·
IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/13)

## OpenID Connect "Session Management" \*\*

[[\[Sign Out / SM\] OpenID Connect \"Session Management\" · Issue \#14 ·
IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/14)

## SAML Single Log Out (SLO) \*\*

[[\[Sign Out / SM\] SAML Single Log Out (SLO) · Issue \#15 ·
IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/15)

## IFrame Based Session Extension \*\*

Would probably belong to sign in, but it's a session management
technique.

[[\[Sign Out / SM\] IFrame-based Session Extension · Issue \#16 ·
IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/16)

# Access Token Acquisition

## OAuth2 Code Flow

Classic OAuth2 flow meant to get an access token on the app backend,
that does NOT result in a RP session (but will create/leverage a AS/OP
session).

[[\[AT\] OAuth 2.0 Code Flow · Issue \#17 · IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/17)

## OAuth2 SPA sign in/access token retrieval via code+PKCE

[[\[AT\] OAuth 2.0 SPA sign in/access token retrieval via code+PKCE ·
Issue \#18 · IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/18)

## OAuth2 SPA sign in/access token retrieval via implicit flow, fragment

[[\[AT\] OAuth 2.0 SPA sign in/access token retrieval via implicit flow,
fragment · Issue \#19 · IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/19)

## OAuth2 SPA background token renewal (iframe)\*\*

\[Code \| implicit\] + prompt=none in an iFrame.

[[\[AT\] OAuth 2.0 SPA background token renewal (iframe) · Issue \#20 ·
IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/20)

## OAuth2 SPA background token renewal (refresh token)

Obtaining an access token via local code+PKCE, refresh token.

[[\[AT\] OAuth 2.0 SPA background token renewal (refresh token) · Issue
\#21 · IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/21)

## OAuth2 JS bearer token usage

Classic API call with an AT (shouldn\'t have issues, but mentioned out
of exhaustiveness).

[[\[AT\] OAuth 2.0 JS bearer token usage · Issue \#22 ·
IDBrowserUseCases/docs
(github.com)]{.ul}](https://github.com/IDBrowserUseCases/docs/issues/22)

# 

# IP1 marks device with cookie to flag it as "trusted"

# 

# Authentication Methods:

-   ## Password

-   ## FIDO

-   ## QR code

-   ## Push to app

-   ## OTP via SMS to phone number

-   ## OTP via Email to email address

-   ## TOTP
