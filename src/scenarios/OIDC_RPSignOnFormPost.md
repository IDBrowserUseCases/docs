## OpenID Connect Redirect Based Sign in via Form POST 
Web application RP1 offers sign in/sign up functionality for users of identity provider IP1, using OpenID Connect implicit flow and form_post. Ignoring how IP1 auths the user, apart from the fact that successful auth results in a cookie in IP1 domain..

### Summary

#### Contributor 
- Name: Vittorio Bertocci
- Organization: Auth0 Inc
- Email: vittorio@auth0.com

#### Protocol
- Name: OpenID Connect
- Grant/flow (if applicable): Implicit flow with form post
- Reference: see the (OpenID Connect Implicit flow)[https://openid.net/specs/openid-connect-core-1_0.html#ImplicitFlowAuth] and (OAuth 2.0 Form Post Response Mode)[https://openid.net/specs/oauth-v2-form-post-response-mode-1_0.html].

#### Browser Features Required
Per legs of the flow:
RP1->IP1  
- 1st party cookie (RP1 saves nonce)
- Redirect
- Decorated links
- 1st party cookie (IP1 saves its session cookie upon successful authentication)  
IP1->RP1
- Form post
- 1st party cookie (nonce carrying cookie)
- Javascript (autopost)

##### Target Audience
This patters applies to every audience, across the board

#### Adoption
TODO Enumeration of products, industries, vendors that rely on this scenario as described.

### Description Of The Flow
TODO long form description of the flow, including start state, end state, and sequence diagram when possible
### Intended User Experience
User navigates to RP1 without any pre existing session in place. Once there, either the user clicks on something (protected route, login button) or the app determines an authentication operation is immediately required. This causes the browser to be redirected to IP1, where the user is presented with authentication prompts (details of the authentication factors/mechanics omitted in this particular scenario). Upon successful authentication, the browser is redirected to RP1, and the user is now signed in RP1. 

### Privacy Considerations
An ID token does transit thru the browser, however
1. It’s not meant for the browser. Format might change. It might be encrypted.
2. It might or might not contain profile user attributes
### Miscellaneous
TODO anything not fitting any of the sections above that is relevant for understanding how the scenario might be affected by browser changes.