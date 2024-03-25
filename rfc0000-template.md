# Request For Comment Template

| Status        | (Proposed / Accepted / Implemented / Rejected / Obsolete) |
:-------------- |:------------------------------------------------------ |
| **RFC #**     | [0000](https://github.com/CRModders/rfcs/pull/0000) (update when you have PR #) |
| **Author(s)** | My Name (me@example.org), Other Name (you@example.org) |
| **Updated**   | YYYY-MM-DD                                             |
| **Obsoletes** | The RFC(s) it replaces. If none, remove this header.   |

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and
"OPTIONAL" in this document are to be interpreted as described in [IEEE RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119).

## Objective
What are we doing and why? What problem will this solve? What are the goals and
non-goals? This is your executive summary; keep it short, elaborate below.

## Motivation
Why is this a valuable problem to solve? What background information is needed to show how this design addresses the problem?

Why is it a problem? What related work exists? Where does it fail to solve the problem?

## User Benefit
How will users (or other contributors) benefit from this work? Are there any drastic changes?

## Design Proposal

This is the meat of the document, where you explain your proposal. If you have multiple alternatives, be sure to use 
sub-sections for better separation of the idea, and list pros/cons to each approach. If there are alternatives that you 
have eliminated, you SHOULD also list those here, and explain why you believe your chosen approach is superior.

Make sure you’ve thought through and addressed the following sections. If a section is not relevant to your specific 
proposal, please explain why, e.g. your RFC addresses a convention or process, not an API.

## Alternatives Considered
* Make sure to discuss the relative merits of alternatives to your proposal.

## Performance Implications
* Do you expect any (speed / memory) degradation? How will you confirm?
* There SHOULD be tests and benchmarks. If there are not, how will you confirm the correctness of your proposal?

## Dependencies
* Dependencies: does this proposal depend on another proposal or project? Which ones?

## Engineering Impact
* Who will maintain the implementation of this proposal? Is this implementation in its own buildable unit? Can this 
implementation be tested in its own?
* Does this proposal require breaking changes to the core API and/or loader? Which ones? Why?

## Best Practices
* Does this proposal change best practices for some aspect of using/developing mods using the current APIs? 
How will these changes be communicated/enforced?

## Tutorials and Examples
* If the design changes existing APIs or creates new ones, the design owner SHOULD create end-to-end examples 
(ideally, a tutorial) which reflects how the new feature will be used. Some things to consider related to the tutorial:
    - It SHOULD NOT require implicit knowledge of other APIs. If implicit knowledge is needed to a reasonable extent,
it SHOULD offer informational sources for such knowledge. If the API requires a great amount of implicit knowledge of 
other APIs, it MUST offer informational sources for such knowledge.
    - It SHOULD show the usage of the new feature in an end to end example. Many new features have unexpected effects in 
parts far away from the place of change that can be found by running through an end-to-end example.
    - It SHOULD be written as if it is documentation of the new feature, i.e., consumable by a developer without prior 
knowledge of the design.
    - The tutorial code MAY NOT work, as the design MAY be lacking a working implementation. However, the code MUST
work after the full implementation of the design.

## Compatibility
* How will this proposal interact with other parts of the Cosmic Reach Modding Ecosystem?
    - How will it work with current mod loaders? Is it fundamentally incompatible with any of them?
    - How will it work with current APIs? Is it fundamentally incompatible with any of them?
    - How will it interact with current mods? Does it require modifications for current codebases?
    - Does it provide backwards compatibility? If not, what changes would be required?

## User Impact
* What are the user-facing changes? How will this feature be rolled out? Will there be drastic changes for 
non-developers?

## Detailed Design

This section is optional. Elaborate on details if they’re important to
understanding the design, but would make it hard to read the proposal section
above.

## Questions and Discussion Topics

Seed this with open questions you require feedback on from the RFC process.
