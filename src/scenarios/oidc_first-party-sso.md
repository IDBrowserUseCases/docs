## OpenID Connect - First Party SSO
Web property with multiple services that all use the same identity provider.

### Summary

##### Contributor
- Name: George Fletcher
- Organization: Verizon Media
- Email: gffletch@aol.com

#### Protocol
- Name: OIDC
- Grant/flow (if applicable): AuthCode
- Reference: [aa](https://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth)

#### Browser Features Required
- 1st party Cookie
- 3rd party Cookie (cross-domain use case)
- Redirect with link decoration
- Local Storage
- JavaScript

##### Target Audience
B2C

#### Adoption
Unknown but I suspect this pattern is widely used.

### Description Of The Flow
Provide single-sign-on services across a set of web domains managed by the same company. Take for example a company named Acme that runs multiple services in different geographical locations with multiple domains: service1.acme.example, service2.acme.<country-domain>, service3.roadrunner.example. To manage the identity services across all these properties Acme runs idp.acme.example.

Using OpenID Connect (OIDC) Acme can allow each property to request the user to authenticate as needed, and the user's single authentication can be shared across all the different properties.

Generally, the user will visit a property (service1.acme.example) and either select the login option (or be immediately) redirected to idp.acme.example. As part of the HTTP redirect, service1.acme.example will set a cookie with an encrypted value containing the secret used to generate the OIDC state parameter (SHA256 has of the secret) and the value of the nonce parameter. This cookie should be explicitly set to samesite=lax and written on the service1.acme.example domain.

When the user arrives at the login page, idp.acme.example will determine if the user is already logged in and if so, immediately redirect the user to service1.acme.example with the OIDC authorization code and state values. If the user is not currently logged in, then the browser will display UI requesting the user to login. This UI uses Javascript to determine elements of the browser and possibly stored state. The user will be asked to enter their authentication credentials. Once the user has authenticated, the browser will be redirected to service1.acme.example with the authorization code and state values.

When service1.example.com receives the redirect from idp.acme.example, it will first look for the encrypted cookie value it wrote when requesting the user to authenticate. It will then validate the state value to ensure this redirect is coming from the same browser where the request was initiated. Service1.example.com will also extract the nonce value for comparison with the nonce in the id_token when it is retrieved. Note that if the state parameter validation fails, service1.acme.example will abort the authentication with an error.

The rest of the flow is standard OAuth/OpenID Connect for obtaining tokens and validating them (e.g. id_token nonce validation). Now that service1.acme.example has valid tokens and an authenticated user, it writes a session cookie on the services1.acme.example domain identifying the user who authenticated. This cookie should also be set with the samesite=lax attribute. Note that it is out of scope for this description as to how the authentication session is referenced in the service1 specific cookie. However, it is important to note that just writing the session cookie on the *.acme.example domain may not be desired because the allowed authorization access of the user at service1.acme.com may be different than the allowed authorization access at service4.acme.com.

#### Sequence Diagram
```
                    ┌───────┐            ┌─────────────────────┐          ┌────────────────┐            ┌───────────────────────────┐
                    │browser│            │service1.acme.example│          │idp.acme.example│            │service3.roadrunner.example│
                    └───┬───┘            └──────────┬──────────┘          └───────┬────────┘            └─────────────┬─────────────┘
              ╔═════════╧═══════════════════════════╧═════════════════════════════╧═══════════════════════════════════╧═════════╗    
              ║Sign-in Process                                                                                                 ░║    
              ╚═════════╤═══════════════════════════╤═════════════════════════════╤═══════════════════════════════════╤═════════╝    
                        │   Click Sign-in Button    │                             │                                   │              
                        │──────────────────────────>│                             │                                   │              
                        │                           │                             │                                   │              
                        │[redirect] set CSRF cookie │                             │                                   │              
                        │samesite=lax               │                             │                                   │              
                        │<─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ │                             │                                   │              
                        │                           │                             │                                   │              
                        │                  Start authentication                   │                                   │              
                        │────────────────────────────────────────────────────────>│                                   │              
                        │                           │                             │                                   │              
                        │                           │                             │────┐                                             
                        │                           │                             │    │ Is the user already logged in?              
                        │                           │                             │<───┘                                             
                        │                           │                             │                                   │              
                        │                           │                             │  ╔═════════════════════════════╗  │              
                        │                           │                             │  ║Validate User AuthN cookies ░║  │              
                        │                           │                             │  ╚═════════════════════════════╝  │              
                        │                           │                             │                                   │              
          ╔══════╤══════╪═══════════════════════════╪═════════════════════════════╪═══════════════════════════════════╗              
          ║ ALT  │  User is NOT logged in           │                             │                                   ║              
          ╟──────┘      │                           │                             │                                   ║              
          ║             │              Request user to authenticate               │                                   ║              
          ║             │<────────────────────────────────────────────────────────│                                   ║              
          ║             │                           │                             │                                   ║              
          ║             │                    User credentials                     │                                   ║              
          ║             │────────────────────────────────────────────────────────>│                                   ║              
          ║             │                           │                             │                                   ║              
          ║             │                           │                             │────┐                              ║              
          ║             │                           │                             │    │ Validate User credentials    ║              
          ║             │                           │                             │<───┘                              ║              
          ║             │                           │                             │                                   ║              
          ║             │           [redirect] Set User AuthN cookies             │                                   ║              
          ║             │<─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ │                                   ║              
          ╠═════════════╪═══════════════════════════╪═════════════════════════════╪═══════════════════════════════════╣              
          ║             │                           │                             │                                   ║              
          ║             │                       [redirect]                        │                                   ║              
          ║             │<─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ │                                   ║              
          ╚═════════════╪═══════════════════════════╪═════════════════════════════╪═══════════════════════════════════╝              
                        │                           │                             │                                   │              
                        │Authorization Code + state │                             │                                   │              
                        │──────────────────────────>│                             │                                   │              
                        │                           │                             │                                   │              
                        │                           │       request tokens        │                                   │              
                        │                           │────────────────────────────>│                                   │              
                        │                           │                             │                                   │              
                        │                           │ access, refresh, id tokens  │                                   │              
                        │                           │<────────────────────────────│                                   │              
                    ┌───┴───┐            ┌──────────┴──────────┐          ┌───────┴────────┐            ┌─────────────┴─────────────┐
                    │browser│            │service1.acme.example│          │idp.acme.example│            │service3.roadrunner.example│
                    └───────┘            └─────────────────────┘          └────────────────┘            └───────────────────────────┘
```
### Intended User Experience
The goal of this user experience is to allow the user to share their authentication across all the properties managed by Acme.

### Privacy Considerations
Since all parties involved in this flow are services offered by Acme, the user's privacy is covered under Acme's privacy policy.

### Miscellaneous
This flow description is just to cover the redirect aspect of the sign-in experience. Other use cases will cover additional aspect such as how the IDP can associate trust with the user's authentication request, or how to help the user address the NASCAR issue.
