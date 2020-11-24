# Contributing to the Catalog of Identity Use Cases Leveraging Web Browsers

Before submitting feedback and contributions, please familiarize yourself with our current issues list and review the [working
group home page](https://datatracker.ietf.org/wg/oauth/documents/). If you're
new to this, you may also want to read the [Tao of the
IETF](https://www.ietf.org/tao.html).

Be aware that all contributions to the specification fall under the "NOTE WELL"
terms outlined below.

## Contribution Process

This informational draft is meant to be a living document, where new scenarios and refinement of the current ones will keep being added as needed.
This work originated in the [IETF working group for OAuth](https://datatracker.ietf.org/wg/oauth/documents/), however we hope to gather contributions from the entire identity community regardless of affiliation. For details on the formal contribution process, please refer to [CONTRIBUTING.md](CONTRIBUTING.md).
Most of the work will happen on https://github.com/IDBrowserUseCases/docs, a GitHub repository meant to facilitate contributions from the community as summarized below. 

### Contributing Scenarios

Each scenario is captured in a document following the template described in {{usecasestemplate}}.
You can find all contributed scenarios in [docs/src/scenarios](https://github.com/IDBrowserUseCases/docs/tree/main/src/).
If you want to contribute a new scenario:
1. Please check [CONTRIBUTING.md](CONTRIBUTING.md) and ensure that you meet all requirements
2. Verify that your scenario isn't already captured in any of the documents in [docs/src/scenarios](https://github.com/IDBrowserUseCases/docs/tree/main/src/scenarios). If it is a variant of an already captured scenario, please consider bringing it up on the [mailing list](https://www.ietf.org/mailman/listinfo/oauth) instead.
3. If your scenario is new, please create a local copy of [SCENARIOTEMPLATE.md]() 
4. Edit your local copy by filing the appropriate sections of the template, see {{usecasestemplate}} for details.
5. Once you are ready, please rename your file to match the format `protocolname_protocolscenario.md` and submit a PR to add it to the scenarios folder
6. Monitor the [mailing list](https://www.ietf.org/mailman/listinfo/oauth) for discussions and requests for clarification

### Discussing Scenarios Details and Inclusion

All submissions will be discussed on the [mailing list](https://www.ietf.org/mailman/listinfo/oauth), possibly undergoing revisions according to the feedback, and once rough consensus is reached, the scenario will be included in this informational document. 


# NOTE WELL

Any submission to the [IETF](https://www.ietf.org/) intended by the Contributor
for publication as all or part of an IETF Internet-Draft or RFC and any
statement made within the context of an IETF activity is considered an "IETF
Contribution". Such statements include oral statements in IETF sessions, as
well as written and electronic communications made at any time or place, which
are addressed to:

 * The IETF plenary session
 * The IESG, or any member thereof on behalf of the IESG
 * Any IETF mailing list, including the IETF list itself, any working group 
   or design team list, or any other list functioning under IETF auspices
 * Any IETF working group or portion thereof
 * Any Birds of a Feather (BOF) session
 * The IAB or any member thereof on behalf of the IAB
 * The RFC Editor or the Internet-Drafts function
 * All IETF Contributions are subject to the rules of 
   [RFC 5378](https://tools.ietf.org/html/rfc5378) and 
   [RFC 3979](https://tools.ietf.org/html/rfc3979) 
   (updated by [RFC 4879](https://tools.ietf.org/html/rfc4879)).

Statements made outside of an IETF session, mailing list or other function,
that are clearly not intended to be input to an IETF activity, group or
function, are not IETF Contributions in the context of this notice.

Please consult [RFC 5378](https://tools.ietf.org/html/rfc5378) and [RFC 
3979](https://tools.ietf.org/html/rfc3979) for details.

A participant in any IETF activity is deemed to accept all IETF rules of
process, as documented in Best Current Practices RFCs and IESG Statements.

A participant in any IETF activity acknowledges that written, audio and video
records of meetings may be made and may be available to the public.
