# Browser-Dependent Identity Use Cases 

This repo is meant as working space for members of the identity protocols community to collaborate on documenting the ways in which identity protocols (modern and otherwise) rely on web browser features to achieve classic identity scenarios, use cases and functionality. 

## Qs
- What format should the doc follow? Is it an internet draft? Should we use rfc xml or markup?
- What structure shoukd we use? Would like to maximize parallelization
- related to the above. SHould we have one doc, or a collection of docs (eg one for each scenarios) so that we avoid merge conflicts, have clear owners etc?

## Brain dump

Primitives
- cookies for tracking session, preferences, transient user agent data (eg nonce)
- redirects to hand over control/experience from app to provider (IdP and/or fed provider)
- iframes for background operations
- XHR + CORS for code redemption, RTs
- QS for parameter passing
- POST for auth, forms, large payloads

Scenarios
- redirect based flows from the RP standpoint (nonce cookie; session cookie in response to a POST)
- SSO (AS cookie)
- background token refresh; session management
- federation providers (not just enterprise)
- "unusual" auth methods
- multiple simultaneous sessions (eg SPA calling dropbox and google drive API)
- backchannel flows
- opaque tokens

## Actions

- define template
---- Protocol/grant
---- Privacy considerations
---- Description fo the flow
---- B2C/B2E/B2B
---- 
