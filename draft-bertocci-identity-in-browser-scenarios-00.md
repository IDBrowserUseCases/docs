---
title: Identity Use Cases in Browser Catalog
abbrev: TODO - Abbreviation
docname: draft-todo-your-name-here
category: info

ipr: trust200902
area: General
workgroup: oauth
keyword: Internet-Draft

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: V.
    name: Vittorio Bertocci
    organization: Auth0
    email: vittorio@auth0.com
 -
    ins: G.
    name: George Fletcher
    organization: Verizon Media
    email: gffletch@aol.com

normative:
  RFC2119:

informative:



--- abstract

This informational document aims to gather in a single place all the most important scenarios in which identity protocols in current use leverage web browser features to achieve their goals and deliver their intended user experience.
The purpose of compiling this scenario collection is to make it easier for the identity community to engage with the browser vendors, and in particular to preserve (or enhance) user experience and expressive power of the identity protocols in mainstream use as browsers introduce new privacy preserving restrictions and new identity tailored features.
By providing a single artifact, listing scenarios in a consistent format, we hope to anchor the conversation on concrete outcomes and impact of changes on end users, developers, providers and in general everyone contributing to identity in the industry.

--- middle

# Overview

As attempts to profile and track user activities on the web intensify, leading to increasingly egregious privacy violations, browser vendors introduce new constraints meant to thwart known tracking techniques.
As they do so, however, they often end up breaking legitimate use cases as well- with identity protocols features such as single sign on, token renewals and the like being disptoportionally affected.
Conscious of those effects and committed to preserve user experience, browser vendors are working on dedicated identity API that aim at preserving and enhancing the user experience in identity transactions, without relying on the general purpose artifacts on whihc current identity protocols depend on. 
One key challenge that is emerging in the process is that browser vendors tend to design around a limited set of well-known, consumer-only use cases, classifying most other cases as enterprise use cases hence solvable by exceptions and local business policies, whereas that is often not the case (e.g., single sign on is a common requirements across web properties even for consumer services, such as online managines form the same publisher) or the expected solutions (e.g. companies deploying MDM and managing policies on their employees and controbuters machines) are not always viable. Discussions between browser vendors and identity experts are not always easy, and are frequently repeated whenever the individuals and initiatives involved change. This makes progress difficult.
This infomational document is a collection of use cases in which identity protocols depend on web browser features to perform their intended function. By gathering the main use cases in a single, shared artifact, and by describing every use case thru a fixed schema designed to surface the most salient characteristics germane to the identity-browser features discussion, we aim to provide a tool to facilitate conversations between browser vendors and identity experts.
This is meant to be a living document, constantly gathering and discussing scenarios contribuitions (via dedicated GitHub repository and OAuth working group mailing list) and periodically incorporating new entries in the main document. The contribution process is described in {{contributionprocess}}. 

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
document are to be interpreted as described in BCP 14 {{RFC2119}} {{!RFC8174}}
when, and only when, they appear in all capitals, as shown here.

# Contribution Process {#contributionprocess}

This informational draft is meant to be a living document, where new scenarios and refinement of the current ones will keep being added as needed.
This work originated in the [IETF working group for OAuth](https://datatracker.ietf.org/wg/oauth/documents/), however we hope to gather contributions from the entire identity community regardless of affiliation. For details on the formal contribution process, please refer to [CONTRIBUTING.md](CONTRIBUTING.md).
Most of the work will happen on https://github.com/IDBrowserUseCases/docs, a GitHub repository meant to facilitate contributions from the community as summarized below. 

## Contributing Scenarios

Each scenario is captured in a document following the template described in {{usecasestemplate}}.
You can find all contributed scenarios in [docs/src/scenarios](https://github.com/IDBrowserUseCases/docs/tree/main/src/).
If you want to contribute a new scenario:
1. Please check [CONTRIBUTING.md](CONTRIBUTING.md) and ensure that you meet all requirements
2. Verify that your scenario isn't already captured in any of the documents in [docs/src/scenarios](https://github.com/IDBrowserUseCases/docs/tree/main/src/scenarios). If it is a variant of an already captured scenario, please consider bringing it up on the [mailing list](https://www.ietf.org/mailman/listinfo/oauth) instead.
3. If your scenario is new, please create a local copy of SCENARIOTEMPLATE.md [[TODO add path]]
4. Edit your local copy by filing the appropriate sections of the template, see {{usecasestemplate}} for details.
5. Once you are ready, please rename your file to match the format <protocolname>_<protocolscenario>.md and submit a PR to add it to the scenarios folder
6. Monitor the [mailing list](https://www.ietf.org/mailman/listinfo/oauth) for discussions and requests for clarification

## Discussing Scenarios Details and Inclusion

All submissions will be discussed on the [mailing list](https://www.ietf.org/mailman/listinfo/oauth), possibly undergoing revisions according to the feedback, and once rough consensus is reached, the scenario will be included in this informational document. 

# The Use Case Template {#usecasestemplate}

Details on how to use the template.

# Use Cases {#usecases}

Empty for now




# Security Considerations

This document has no security considerations. Readers should refer to the security considerations of the specifications references by each of the individual use cases.

# IANA Considerations

This document has no IANA actions.



--- back

# Acknowledgments
{:numbered="false"}

Thanks to everyone who helped create the tools that let us use Markdown to create 
Internet Drafts.