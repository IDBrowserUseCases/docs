## RP-Initiated Front-Channel Logout 

### Summary

RP-Initiated Front-Channel Logout is used by OpenID Providers as part of a sign out flow to clear session cookies for Relying Parties (RPs), without requiring major frontend or backend changes to the RP.

#### Contributor 
- Name: Tim Cappalli
- Organization: Microsoft Identity
- Email: tim.cappalli@microsoft.com

#### Protocol
- Name: OpenID Connect
- Grant/flow (if applicable): RP-Initiated Front-Channel Logout
- Reference: 
  - [Spec: OpenID Connect RP-Initiated Logout 1.0](https://openid.net/specs/openid-connect-rpinitiated-1_0.html)
  - [Spec: OpenID Connect Front-Channel Logout 1.0](https://openid.net/specs/openid-connect-frontchannel-1_0.html)
  - [Presentation: What Does Logout Mean?](https://self-issued.info/presentations/What_Does_Logout_Mean_Presentation.pdf)

#### Browser Features Required
- 1st party cookies
- 3rd party cookies
- Redirect
- Link decoration
- iframes
- JavaScript

##### Target Audience

This flow applies to all OpenID Connect deployments where central logout is desired and RPs cannot host an OP's iframe.

#### Adoption

The following services are known to support OIDC Front-Channel Logout:
* Microsoft Azure AD (IdP/OP)
* Microsoft 365 (RP)
* Microsoft Active Directory Federation Services (IdP/OP)
* PingFederate (IdP/OP)
* Okta (IdP/OP)
* Duende IdentityServer (IdP/OP)
* Gluu Server (IdP/OP)
* Salesforce (IdP/OP)
* Salesforce (RP)


### Description Of The Flow

1. User clicks log out (or otherwise invokes a log out event) on the active relying party (A), resulting in a user agent call to the relying party (A).
2. The relying party (A) may reach out to the OpenID Provider's metadata endpoint to determine the end session endpoint. 
3. The metadata JSON object is returned.
4. The relying party's response is either a 302 redirect to the end session endpoint or an HTML page containing JavaScript to initiate the redirect. The response often includes Set-Cookie headers to reset session cookies.
5. The user agent navigates to the OP's end session endpoint. To identify the session, a `sid` or `id_token_hint` query parameter may be included in the URL or cookies sent with the request may be used. An `iss` query parameter may be included to provide additional scope to the `sid`. An optional `post_logout_redirect_uri` query parameter may be included if a redirect back to the relying party after log out is desired.
6. The OP responds with an HTML page containing information for the user and hidden iframes for the replying party log out endpoints where there are known active sessions.
7. The User Agent loads the first iframe and calls RP B's log out endpoint. 
8. A 200 is returned with Set-Cookie headers to reset session cookies for RP B.
9. The User Agent loads the second iframe and calls RP C's log out endpoint. 
10. A 200 is returned with Set-Cookie headers to reset session cookies for RP C.
11. The OP/IdP may redirect the user agent back to RP A, often using JavaScript.

#### Sequence Diagram

```
                              ,----------.                           ,-------------------------.          ,----.          ,----.          ,-----------------.     
                              |User Agent|                           |Relying Party A (Primary)|          |RP B|          |RP C|          |Identity Provider|     
                              `----+-----'                           `------------+------------'          `-+--'          `-+--'          `--------+--------'     
                                   |              1 GET /app/SignOut              |                         |               |                      |              
                                   | --------------------------------------------->                         |               |                      |              
                                   |                                              |                         |               |                      |              
                                   |                                              |             2 GET /.well-known/openid-configuration            |              
                                   |                                              | --------------------------------------------------------------->              
                                   |                                              |                         |               |                      |              
                                   |                                              |                      3 [200]  JSON Object                      |              
                                   |                                              | <- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -              
                                   |                                              |                         |               |                      |              
                                   | 4 [200] HTML with JS redirect - reset cookies|                         |               |                      |              
                                   | <- - - - - - - - - - - - - - - - - - - - - - -                         |               |                      |              
                                   |                                              |                         |               |                      |              
                                   |                                         5 GET {{end_session_endpoint}} |               |                      |              
                                   | -------------------------------------------------------------------------------------------------------------->              
                                   |                                              |                         |               |                      |              
                           ,-------------------------------------------------------------------------------------------------------------------------------.      
                           |?sid=abc123                                                                                                                    |      
                           | &iss=https://login.idp.example/                                                                                               |      
                           | &post_logout_redirect_uri=https://myapp.rp-a.example/home                                                                     |      
                           `-------------------------------------------------------------------------------------------------------------------------------'      
                                   |                                        6 HTML with hidden child iframes|               |                      |              
                                   | <- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -               
                                   |                                              |                         |               |                      |              
                                   |                                              |                         |               |                      |              
          _______________________________________________________________________________________________________________________________________________________ 
          ! IDP PARENT FRAME  /    |                                              |                         |               |                      |             !
          !________________,-----------------------------------------------------------------------------------------------------------------------------!.      !
          !                |<iframe src="{{rp-b}}/logout?sid=abc123" style:"display:none">                                                               |_\     !
          !                |<iframe src="{{rp-c}}/logout?sid=abc123" style:"display:none">                                                                 |     !
          !                `-------------------------------------------------------------------------------------------------------------------------------'     !
          !                        |                                              |                         |               |                      |             !
          !         _____________________________________________________________________________________________________   |                      |             !
          !         ! IDP CHILD IFRAME 1  /                                       |                         |            !  |                      |             !
          !         !____________________/        7 IdP child iframe [ GET /logout?sid=abc123 ]             |            !  |                      |             !
          !         !              | ----------------------------------------------------------------------->            !  |                      |             !
          !         !              |                                              |                         |            !  |                      |             !
          !         !              |                            8 200, Set-Cookie |                         |            !  |                      |             !
          !         !              | <- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -            !  |                      |             !
          !         !              |                                              |                         |            !  |                      |             !
          !         !          ,--------------------------------------------------------------------------------!.       !  |                      |             !
          !         !          |set-cookie: ActiveSession=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/       |_\      !  |                      |             !
          !         !~~~~~~~~~~`----------------------------------------------------------------------------------'~~~~~~!  |                      |             !
          !                        |                                              |                         |               |                      |             !
          !                        |                                              |                         |               |                      |             !
          !         _____________________________________________________________________________________________________________________          |             !
          !         ! IDP CHILD IFRAME 2  /                                       |                         |               |            !         |             !
          !         !____________________/                9 IdP child iframe [ GET /logout?sid=abc123 ]     |               |            !         |             !
          !         !              | --------------------------------------------------------------------------------------->            !         |             !
          !         !              |                                              |                         |               |            !         |             !
          !         !              |                                   10 200, Set-Cookie                   |               |            !         |             !
          !         !              | <- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -            !         |             !
          !         !              |                                              |                         |               |            !         |             !
          !         !          ,------------------------------------------------------------------------------------------------!.       !         |             !
          !         !          |set-cookie: ActiveSession=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/                       |_\      !         |             !
          !         !~~~~~~~~~~`--------------------------------------------------------------------------------------------------'~~~~~~!         |             !
          !~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~!
                                   |                                              |                         |               |                      |              
                                   |                 11 GET /home                 |                         |               |                      |              
                                   | --------------------------------------------->                         |               |                      |              
                              ,----+-----.                           ,------------+------------.          ,-+--.          ,-+--.          ,--------+--------.     
                              |User Agent|                           |Relying Party A (Primary)|          |RP B|          |RP C|          |Identity Provider|     
                              `----------'                           `-------------------------'          `----'          `----'          `-----------------'     

```


### Intended User Experience

A user has been accessing multiple services throughout the day (RP A, RP B, RP C), all of which were authenticated via the same Identity Provider / OpenID Provider and the user experienced a seamless, single sign-on (SSO) experience.

The user is done for the day and wants to logout. In the currently active service, RP A, the user clicks their profile icon and then "Log Out". Their browser is redirected to the Identity Provider. In the background, ther user's other active sessions are cleared at RP B and RP C. The user is then informed that log out is complete and they  can either close the browser or be redirected back to the original service (RP A).

When the user attempts to access RP A, RP B, or RP C again, they should be redirected to the identity provider to start a new session.

### Privacy Considerations
Session cookies used with the cross-origin logout iframes need to be set with SameSite=none.

### Miscellaneous