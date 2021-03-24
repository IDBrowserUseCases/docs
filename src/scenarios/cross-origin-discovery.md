## Protocol-Independent Cross-Origin Domain IdP Discovery
Supporting the use of a central IdP Discovery and Persistence service in a multilateral federation scenario.

### Summary

#### Contributor 
- Name: Heather Flanagan
- Organization: Spherical Cow Consulting
- Email: hlf@sphericalcowconsulting.com
- Name: Nicole Roy
- Organization: University Corporation for Advanced Internet Development dba Internet2
- Email: nroy@internet2.edu
- Name: Leif Johansson
- Organization: SUNET
- leifj@sunet.se

#### Protocol
- n/a

#### Browser Features Required
- cross-origin access through POST message
- iFrames

##### Target Audience
This is a use case currently and very actively in use in the academic community, including scholarly resource providers such as Elsevier and SpringerNature (publishers) and academic identity federation services such as the GÉANT Trusted Certificate Service.

#### Adoption
Higher Education, Scholarly Publishing, Federation Certificate Services. See:

| Org | URL |
| --- | --- |
| American Chemical Society (ACS) | pubs.acs.org |
| Elsevier's ScienceDirect | www.sciencedirect.com |
| GÉANT Trusted Certificate Service (TCS) |wiki.geant.org - click on platform links |
| Nature.com | nature.com |
| SAFIRE Test Service Provider | testsp.safire.ac.za | SUNET | edusign.sunet.se |
| SWAMID | wiki.swamid.se - click on Login button |
| Taylor & Francis | https://www.tandfonline.com/doi/full/10.1080/00049158.2020.1819009 |
| Wiley Online Library | onlinelibrary.wiley.com |


### Description Of The Flow
A full design of the data flows used to support a central discovery service are available here: <https://docs.google.com/presentation/d/1h3XQ6BtTQ7KJkBQkIkWjktp3RXqHDXEKsa5ymrjzA-k/edit#slide=id.p> 

This flow writes to browser local storage via a dedicated component which loads a non-displayed iFrame.


### Intended User Experience
SeamlessAccess allows SPs to populate a common login button with the user’s previous choice of IdP as found in their browser’s local storage. The goal is to minimize the number of times a user has to go through an IdP discovery process. The IdP is identified by a public descriptor that can’t be used for user tracking - in the case of SAML a so-called entityID (which is essentially the public URL of the IdP). Browser local storage does not contain any PII or trackable information.

The goal of this flow is to enable users to have a privacy-preserving, non-tracked method to log into a web resource using their institutional credentials, with a very simple and straightforward UX that looks to the user a lot like the “Log In With X” button, but avoids a recurring “NASCAR” problem by being backed with an IdP discovery service based on search, so that the user may find their IdP once, and then be automatically logged in with that one IdP in the future, without IdPs or RPs being able to collude to gain information about the user. As such, the flow persists the user’s IdP choice in browser local storage. The user can easily change their IdP selection whenever they want.

### Privacy Considerations
The information that is persisted following the IdP discovery flow is not PII (e.g., an email address or a personal identifier) but is typically the public identifier of the IdP (i.e., the entityID). Further, that information is persisted in the user’s own browser local storage.

### Miscellaneous
In some situations, the institution hosting the IdP controls the consent for information release; RPs are not allowed to directly ask the user for information. For instance, GDPR section 6 lists the set of conditions under which personal information may be processed. One of those is “free and informed consent”. In most cases where the data subject is acting outside the purely personal sphere (as in consumer identity), consent cannot be freely given. For instance, if a grad student is tasked to write a piece of code and the official policy is to use GitHub then the GitHub consent screen is arguably illegal in the EU since the user is not able to deny consent. Similar situations arise in all federation use-cases and it is for this reason very uncommon for R&E services in the EU to rely on consent as a legal foundation for processing PII.