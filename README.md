# CRModders RFCs
The "RFC" (request for comments) process is intended to provide a consistent and controlled path for changes to the 
projects developed within the CRModders organization in a way that satisfies the Cosmic Reach modding community as a
whole.

Many changes, including bug fixes and documentation improvements can be implemented and reviewed via the normal GitHub 
pull request workflow.

## Table Of Contents
* [Table Of Contents](#table-of-contents)
* [When you need to follow this process](#when-you-need-to-follow-this-process)
* [The RFC Process](#the-rfc-process)

## When you need to follow this process
You need to follow this process if you intend to make "substantial" changes to any major project that is considered a 
standard, or the RFC process itself. What constitutes a "substantial" change evolves based on community norms and varies 
depending on what part of the ecosystem you are proposing to change, and may include, but not limited to the following:
- Any changes that modify a major part of a mod loader, a launcher or a Core API.
- Removing features from one of the aforementioned projects.
- Any changes to the public interfaces for modding APIs.
- Removal or other changes that greatly reduce the documentation for a given project.

Some changes that do not require an RFC:
- Rephrasing, reorganizing, refactoring, or otherwise "changing shape does not change meaning".
- Additions or clarification to the documentation for a given project.
- Additions only likely to be noticed by other developers of a project, invisible to outside users of said project.

If you submit a pull request to implement a new feature without going through the RFC process, it may be closed with a 
polite request to submit an RFC first.

## Before creating an RFC
A hastily-proposed RFC may reduce its chances of acceptance. Low quality proposals, proposals for previously-rejected 
features, or proposals that don't fit into the near-term roadmap, may be quickly rejected. Laying some groundwork ahead 
of the RFC can make the process smoother.

Although there is no single way to prepare for submitting an RFC, it is generally a good idea to pursue feedback from 
other project developers beforehand, to ascertain that the RFC may be desirable. At the time of writting, a good place 
to request feedback is the [CRModders Discord](https://crmodders.dev/discord), more specifically, the `mod-ideas` 
forum channel.

## The RFC Process
In short, the process to make changes follows the steps detailed below. From the moment an RFC is accepted into the 
repository, the RFC is "active" and may be implemented with the goal of it's eventual inclusion.
- Fork the [RFC repository](https://github.com/CRModders/rfcs).
- Copy [0000-template.md](./rfc0000-template.md) to proposals/rfc-desc.md (where "desc" is a short description of the 
proposal). Don't assign an RFC number yet; This is going to be the PR number and we'll rename the file accordingly if 
the RFC is accepted.
- Fill in the RFC. Put care into the details: RFCs should provide enough information for a reader to understand the 
basic functionality of the proposed design.
- Submit a pull request. As a pull request the RFC will receive design feedback from the larger community, and the 
author should be prepared to revise it in response.
- Being accepted, the RFC becomes "active" and the pull request number will be added to the proposal file.
- Build consensus and integrate feedback. Discuss on the discord server and rectify the RFC as required. You may make 
edits, big and small, to the RFC to clarify or change the design, but make changes as new commits to the pull request, 
and leave a comment on the pull request explaining your changes.
- After recieving and implementing feedback and reaching a consensus with members of the community, you may request a
review for the RFC. After being reviewed, the RFC status will be changed to one of the following:
  - Accepted: The RFC was accepted and will be implemented on the next release of it's corresponding project.
  - Rejected: The RFC was rejected, and given a reason for it's rejection.
- After being accepted, the RFC will pass to the Implementation Phase, at which point the changes are greenlit and 
accepted into it's corresponding project's codebase, at which point the RFC status will be changed to "Implemented". 
Eventual minor changes might be required for a successful merge, which MUST NOT require a major change to the RFC itself. 
In such cases, the RFC must be reclassified as "Proposed" and rectified.
- At some point in the future, a new RFC may propose a substantial change, restructuring or complete removal of the 
implementation of an RFC. If accepted, the superseded proposal will have it's status changed to "Obsolete" and a link to
the current proposal will be added.