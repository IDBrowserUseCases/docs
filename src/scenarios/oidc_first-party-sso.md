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

When the user arrives at the login page, idp.acme.example will determine if the user is already logged in or not and if so, redirect the user to service1.acme.example with the OIDC authorization code and state values. If the user is not currently logged in, then the browser will display UI requesting the user to login. The UI will use JavaScript to determine if any users have logged into idp.acme.example from this browser in the past and if so present an "account chooser" experience. If not the user will be asked to enter their authentication credentials. Once the user has authenticated, the browser will be redirected to service1.acme.example with the authorization code and state values.

When service1.example.com receives the redirect from idp.acme.example, it will first look for the encrypted cookie value it wrote when requesting the user to authenticate. It will then validate the state value to ensure this redirect is coming from the same browser where the request was initiated. Service1.example.com will also extrace the nonce value for comparison with the nonce in the id_token when it is retrieved. Note that if the state parameter validation fails, service1.acme.example will abort the authentication with an error.

Once the state parameter has been validated, service1.acme.example will exchange the authorization code with idp.acme.example presenting it's own client credentials to prove it is the correct entity to be exchanging the authorization code for tokens. idp.acme.example will validate the client credentials and authorization code and if valid return an access token, optionally a refresh token and an id token per the OpenID Connect spec.

Service1.acme.example will validate the id_token and verify that the nonce in the id_token matches the nonce extracted from the encrypted cookie. Now that service1.acme.example has valid tokens and an authenticated user, it writes a session cookie on the services1.acme.example domain identifying the user who authenticated. This cookie should also be set with the samesite=lax attribute. Note that it is out of scope for this description as to how the authentication session is referenced in the service1 specific cookie. However, it is important to note that just writing the session cookie on the *.acme.example domain may not be desired because the allowed authorization access of the user at service1.acme.com may be different than the allowed authorization access at service4.acme.com.

### Intended User Experience
The goal of this user experience is to allow the user to share their authentication across all the properties managed by Acme. This scenario does not cover the use cases where the user is "seamlessly" recognized as being signed in and then silently authenticated to the site. That requires it's own use case document:)

### Privacy Considerations
In order to provide the best user experience, idp.acme.example must be able to remember who has logged in from this browser in the past. This does leave some user data in the browser (i.e. local storage and persistent cookies) that will be used during the user's next visit to streamline the user experience. Most often this knowledge of what has happened in the past can be used by idp.acme.example to increase the concept of trust associated with this authentication event.

### Miscellaneous
[todo] anything not fitting any of the sections above that is relevant for understanding how the scenario might be affected by browser changes.
