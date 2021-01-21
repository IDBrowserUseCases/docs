# Overview {#overview}

As attempts to profile and track user activities on the web intensify, leading to increasingly egregious privacy violations, browser vendors introduce new constraints meant to thwart known tracking techniques.
As they do so, however, they often end up breaking legitimate use cases as well- with identity protocols features such as single sign on, token renewals and the like being disptoportionally affected.
Conscious of those effects and committed to preserve user experience, browser vendors are working on dedicated identity API that aim at preserving and enhancing the user experience in identity transactions, without relying on the general purpose artifacts on which current identity protocols depend on. 
One key challenge that is emerging in the process is that browser vendors tend to design around a limited set of well-known, consumer-only use cases, classifying most other cases as enterprise use cases hence solvable by exceptions and local business policies, whereas that is often not the case (e.g., single sign on is a common requirements across web properties even for consumer services, such as online managines form the same publisher) or the expected solutions (e.g. companies deploying MDM and managing policies on their employees and contributors machines) are not always viable. Discussions between browser vendors and identity experts are not always easy, and are frequently repeated whenever the individuals and initiatives involved change. This makes progress difficult.
This infomational document is a collection of use cases in which identity protocols depend on web browser features to perform their intended function. By gathering the main use cases in a single, shared artifact, and by describing every use case thru a fixed schema designed to surface the most salient characteristics germane to the identity-browser features discussion, we aim to provide a tool to facilitate conversations between browser vendors and identity experts.
This is meant to be a living document, constantly gathering and discussing scenarios contribuitions (via dedicated GitHub repository and OAuth working group mailing list) and periodically incorporating new entries in the main document. The contribution process is described in (#contributionprocess). 

## Scope

This document considers in scope scenarios and use cases for which all the following requirements apply:
- must be in common use, represent a common practice, a behavior of products in widespread adoption, etc.
- can be from any mainstream identity and authorization protocol specification, regardless of standard bodies affiliation: e.g. OAuth, OpenID Connect, SAML, etc.
- must require use of browser features (e.g. cookies, redirects, decorated links, HTTP headers, local storage etc) for at least part of its sequence of messages.
- can involve, but isn't limited to: establishing a user session, obtaining credentials and intermediate artifacts (e.g. OAuth2 authorization codes).

The following is considered out of scope:
- any scenario or protocol not currently in mainstream use, regardless of standardization status.
- any scenario not using a web broweser in any capacity.
- proposing new browser behaviors or API, or identity scenarios using hypothetical new browser capabilities. The goal of this project is to document what's already in place, to inform discussions about what's next taking place in forums shared with the browser vendors.


# Conventions and Definitions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this
document are to be interpreted as described in BCP 14 [@RFC2119] [@RFC8174] 
when, and only when, they appear in all capitals, as shown here.

# Contribution Process {#contributionprocess}

Before submitting feedback and contributions, please familiarize yourself with our current issues list and review the [working
group home page](https://datatracker.ietf.org/wg/oauth/documents/). If you're
new to this, you may also want to read the [Tao of the
IETF](https://www.ietf.org/tao.html).

Be aware that all contributions to the specification fall under the "NOTE WELL"
terms outlined [here](https://www.ietf.org/about/note-well/).

This informational draft is meant to be a living document, where new scenarios and refinement of the current ones will keep being added as needed.
This work originated in the [IETF working group for OAuth](https://datatracker.ietf.org/wg/oauth/documents/), however we hope to gather contributions from the entire identity community, regardless of affiliation. 
Most of the work will happen on https://github.com/IDBrowserUseCases/docs, a GitHub repository meant to facilitate contributions from the community as summarized below. 

## Contributing Scenarios

Each scenario is captured in a document following the template described in (#usecasestemplate).
You can find all contributed scenarios in [docs/src/scenarios](https://github.com/IDBrowserUseCases/docs/tree/main/src/).
If you want to contribute a new scenario:
1. Please read the preamble in this section and ensure that you meet all requirements
2. Verify that your scenario isn't already captured in any of the documents in [docs/src/scenarios](https://github.com/IDBrowserUseCases/docs/tree/main/src/scenarios). If it a variant of an already captured scenario, or if you want to contribute to an existing scenario doc, please consider chiming in on the [OAuth mailing list](https://www.ietf.org/mailman/listinfo/oauth).
3. If your scenario is new, please fork the repo and create a local copy of SCENARIOTEMPLATE.md, renaming your file to match the format protocolname_protocolscenario.md
4. Edit your local copy by filing the appropriate sections of the template, see (#usecasestemplate) for details.
5. Once you are ready, please submit a PR to add the new doc/reflect doc updates to the scenarios folder
6. Monitor the [mailing list](https://www.ietf.org/mailman/listinfo/oauth) for discussions and requests for clarification


## Discussing Scenarios Details and Inclusion

All submissions will be discussed on the [mailing list](https://www.ietf.org/mailman/listinfo/oauth), possibly undergoing revisions according to the feedback. Once rough consensus is reached, the scenario will be included in this informational document. 

# The Use Case Template {#usecasestemplate}

We capture all scenarios following a template, with the intent of providing readers with a consistent way of understanding the scenario goals, intended user experience, browser feature dependencies, privacy characteristics and any other consideration that might help assessing impact of browser feature changes on the scenario.
Here's a quick description of each template section. For more details, please refer to the scenarios already captured.

- Scenario Name  
Each scenario document opens with a concise description of the scenario.

- Contributor  
This section captures the individual(s) contributing this scenario document and their affiliation.

- Protocol  
If the scenario is part of a standard (e.g. SAML, OpenID Connect, OAuth2, etc), this section provides details such as what standard document it refers to, and any reference that can be useful to learn more about the scenario. Examples include indicating specific use cases described by the standard (grants, flows, etc), links to articles and presentations describing the scenario in more detail and so on.
Please note that scenarios captured here do not necessarily need to be formally described in a standard, as long as they are in common use. A good example would be the use of cookies for preserving the nonce in OpenID Connect, or the use of cookies for representing a web app session after a federated sign in flow (regardless of the specific protocol) took place. 

- Browser Features Required  
This section provides a quick reference of the browser features that play a role in the correct execution of the scenario.
Examples include 1st and 3rd party cookies, redirects with link decoration, form posts, access to local storage, iFrames, JavaScript, and so on.

- Intended User Experience  
Description of the intended user experience, with particular attention on the desired outcomes (eg no visible prompt in SSO scenarios)

- Description Of The Flow  
Description of the flow, including entities coming into play, begin state, end state, and sequence diagram when possible.

- Privacy Considerations  
Description of privacy characteristics of the scenario, with particular attention to aspects affecting the browser (eg presence of browser-readable artifacts carrying user info, use of global|pairwise|no identifiers, etc).

- Target Audience  
This section is meant to indicate whether the scenario is used prevalently for a particular audience (eg B2C,B2E, B2B, G2C) or if it can be expected to be relevant for more than one category.

- Adoption  
If known or easy to assess, this session enumerates notable products, industries, vendors that rely on the scenario as described.

- Miscellaneous  
Anything not fitting any of the sections above that is relevant for understanding how the scenario might be affected by browser changes.

{{scenarios.md}}

# Acknowledgements {#Acknowledgements}
      

    

# IANA Considerations {#IANA}
      
  This draft includes no request to IANA.
    

# Security Considerations {#Security}
      
This document has no security considerations. Readers should refer to the security considerations of the specifications references by each of the individual use cases.