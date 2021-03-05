Template from Section 4 of [https://datatracker.ietf.org/doc/html/draft-bertocci-identity-in-browser-00](https://datatracker.ietf.org/doc/html/draft-bertocci-identity-in-browser-00)


### Scenario Name

Multilateral SAML Federation User Login Flow


### Contributor

Nicole Roy - University Corporation for Advanced Internet Development dba Internet2

Heather Flanagan - Spherical Cow Group

_$yournamehere $yourorghere_


### Protocol

_If the scenario is part of a standard (e.g.  SAML, OpenID Connect, OAuth2, etc), this section provides details such as what standard document it refers to, and any reference that can be useful to learn more about the scenario.  Examples include indicating specific use cases described by the standard (grants, flows, etc), links to articles and presentations describing the scenario in more detail and so on.  Please note that scenarios captured here do not necessarily need to be formally described in a standard, as long as they are in common use.  A good example would be the use of cookies for preserving the nonce in OpenID Connect, or the use of cookies for representing a web app session after a federated sign in flow (regardless of the specific protocol) took place._

This scenario describes (really, points to other, more detailed descriptions of) “multilateral” SAML-federation flows which use the SAML front-channel bindings defined in OASIS SAML v2.0, specifically the Web Browser SSO Profile, Identity Provider Discovery Profile, and Single Logout Profile contained in [^1]. Additional context for the use of these profiles and flows in the multilateral SAML federation space used in the global research and education sector is specified in [^2], [^3].


### Browser Features Required

_This section provides a quick reference of the browser features that play a role in the correct execution of the scenario. Examples include 1st and 3rd party cookies, redirects with link decoration, form posts, access to local storage, iFrames, JavaScript, and so on._

Low-level primitives in the browser including first- and third-party cookies, persistent browser-local storage, link decoration/ query strings in the URL, and postMessage, are commonly used to simplify the user experience in SAML identity provider discovery and web SSO flows.


### Intended User Experience

_Description of the intended user experience, with particular attention on the desired outcomes (eg no visible prompt in SSO scenarios)_

The user tries to access a protected web resource. They have insufficient authentication/authorization information with the resource needed to proceed. They are prompted to choose their identity provider, and sent there to sign in. Upon successful authentication, they are redirected to the web resource with a SAML token containing authentication, and optionally user profile and authorization information needed to support their access claim. There may be tens, hundreds, or thousands of IdPs the user may choose from. In many real-world scenarios present in the research and academic sector today, there are thousands.


### Description Of The Flow

_Description of the flow, including entities coming into play, begin state, end state, and sequence diagram when possible._

The user attempts to access a web application (“service”) protected by a piece of middleware known as a “Service Provider” or “SP”. This middleware looks for the presence of a signed and/or encrypted SAML authentication/authorization token in the form of a SAML assertion in the user’s state maintained by the SP. If no such token exists, and the user is attempting to access a resource protected by the SP’s policy, the user is redirected to an Identity Provider (“IdP”) Discovery flow such as **[TEMP link to Heather’s scenario for discovery]**[^4]. The user selects their IdP from hundreds or thousands of such IdPs in global multilateral SAML metadata[^5], or if they have saved an IdP selection, they simply click a login button as specified in the previously mentioned discovery flow. In some cases, there is no discovery as far as the user is aware, and they are simply sent to their IdP without being prompted. In any case, they are redirected to their home organization’s IdP along with an optionally-signed authentication request from the SP, in samlRequest argument. At the IdP, the user authenticates using whatever method they have available and that their IdP supports. In some cases, an SP may request (require or prefer) a specific login method as part of its authentication request. This authentication could use WebAuthN, X.509 personal certificate, username/password/push notification, etc. The IdP looks up SAML metadata for the SP which issued the request, and if signed, verifies the signature using the public key associated with a “signing” or “any” use, for the SP, published in metadata.

If the user successfully authenticates, the IdP inspects a combination of: \
1) The list of requested attributes that the SP publishes in metadata

2) Its own default attribute release policy for that SP or an entire class of SPs to which the SP belongs (example: “All Reserach and Scholarship IdPs in my Federation Metadata”[^6]

It then assembles a list of the attributes it needs to release to the SP, queries the user data store for these values, and (optionally, and depending on local/regional/national policies and regulations) prompts the user for their consent to release the values to the SP. If the user approves, a SAML assertion is constructed which can contain things like privacy-preserving unique identifier, email address, name, certain roles, or other things the application needs to authorize the user, provision/maintain their local profile, etc.

The assertion is encrypted using the SP’s public key associated with a key use of “encryption” or “any” in metadata. Some IdP software may use first-party cookies or browser-local storage to persist the user’s session information with it, which can then be recovered, verified by it, and relayed to other SPs for the purposes of single sign-on without heavy-weight IdP server-side session management. The user is redirected back to the SP with their assertion contained in a third-party cookie targeted at the SP. The SP decrypts/verifies the assertion’s genuine-ness and populates the user’s session with the attributes, and redirects the user to the application endpoint they were originally trying to access.


### Privacy Considerations

_Description of privacy characteristics of the scenario, with particular attention to aspects affecting the browser (eg presence of browser-readable artifacts carrying user info, use of global|pairwise|no identifiers, etc)._

User consent, opaque and/or targeted identifiers, policy frameworks such as REFEDS Code of Conduct, SIRTFI, and Research and Scholarship entity category all contribute to the privacy-preserving intent and realization within this scenario. 


### Target Audience

_This section is meant to indicate whether the scenario is used prevalently for a particular audience (eg B2C, B2E, B2B, G2C) or if it can be expected to be relevant for more than one category._

This use case is U2U (user to user) - users should be able to pick what applications they use to collaborate in research and education, so we empower them to do that using strong identity and security practices provided by their home organization and strengthened by global technical and policy frameworks such as eduGAIN[^7]. This use case is extremely active and important within the research and education sector. It has been fundamental to getting multi-institutional research (examples: Gravitational wave discovery by the LIGO Scientific Collaboration[^8], discovery of the Higgs boson by CERN[^9], the discovery of COVID-19 vaccines[^10], etc.) done for twenty years. It is **_ required_** to be supported by grant recipient organizations receiving US federal grants from both the NSF[^11] and NIH[^12], among many other global research funding agencies. 


### Adoption

_If known or easy to assess, this section enumerates notable products, industries, vendors that rely on the scenario as described._

Shibboleth Project

SimpleSAMLphp

Identity Python

Cirrus Identity

Unicon

Internet2/InCommon and all of their sister national research and education networks globally.

The [academic research enterprise of nearly every developed country on the planet, and many developing countries](https://technical.edugain.org/).

US and international funding agencies

US real estate agents

Global pharmaceutical industry

Global academic and research publishers

The US defense industry (federations like “EXOSTAR”, etc.)

….


### Miscellaneous

_Anything not fitting any of the sections above that is relevant for understanding how the scenario might be affected by browser changes._


<!-- Footnotes themselves at the bottom. -->
## Notes

[^1]:
     [https://www.oasis-open.org/committees/download.php/35389/sstc-saml-profiles-errata-2.0-wd-06-diff.pdf](https://www.oasis-open.org/committees/download.php/35389/sstc-saml-profiles-errata-2.0-wd-06-diff.pdf) 

[^2]:
     [https://kantarainitiative.github.io/SAMLprofiles/fedinterop.html](https://kantarainitiative.github.io/SAMLprofiles/fedinterop.html) 

[^3]:
     [https://kantarainitiative.github.io/SAMLprofiles/saml2int.html](https://kantarainitiative.github.io/SAMLprofiles/saml2int.html) 

[^4]:
     [https://github.com/IDBrowserUseCases/docs/pull/2/commits/d88f092663d44478e7d85022c800a6a785b65b0c](https://github.com/IDBrowserUseCases/docs/pull/2/commits/d88f092663d44478e7d85022c800a6a785b65b0c) 

[^5]:
     [https://technical.edugain.org/](https://technical.edugain.org/) 

[^6]:
     [https://refeds.org/category/research-and-scholarship](https://refeds.org/category/research-and-scholarship) 

[^7]:
     https://edugain.org/

[^8]:
     https://www.trustedci.org/ligo

[^9]:
     https://auth.docs.cern.ch/trouble-shooting/edugain-authentication/

[^10]:
     https://incommon.org/news/international-science-community-seeks-help-from-incommon-participants/

[^11]:
     https://incommon.org/federation/nsf-campus-cyberinfrastructure-and-incommon/

[^12]:
     https://incommon.org/news/nih-to-begin-requiring-multifactor-authentication/
