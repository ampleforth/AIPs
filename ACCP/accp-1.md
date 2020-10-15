---
accp: 1
title: ACCP Purpose and Guidelines
status: Implemented
author: Tom Nash <@morelazers>
discussions-to: https://discord.gg/6Amxhs4
created: 2020-09-12
---

## What is an ACCP?

ACCP stands for Ampleforth Configuration Change Proposal. ACCP's are documents to make a case for modifying one of the system configuration variables. The intent is to provide a clear and detailed history behind each configuration change and the rationale behind it at the time it was implemented. The author of the document is responsible for building consensus within the community and documenting dissenting opinions.

## ACCP Rationale

We intend ACCPs to be the primary mechanisms for proposing configuration changes to Ampleforth. Because they are maintained as text files in a versioned repository, their revision history is the historical record of the configuration change proposal.

It is highly recommended that a single ACCP contain a single variable change. The more focused the ACCP, the more successful it is likely to be.

An ACCP must meet certain minimum criteria. It must be a clear and complete description of the proposed variable change.

## ACCP Work Flow

Parties involved in the process are the *author*, the [*AIP editors*](#aip-editors), the [Ampleforth Core Contributors], and the Ampleforth Community.

:warning: Before you begin, vet your idea, this will save you time. Ask the Ampleforth community first if the proposed change is original to avoid wasting time on something that will be rejected based on prior research (searching the Internet does not always do the trick). It also helps to make sure the idea is applicable to the entire community and not just the author. Just because an idea sounds good to the author does not mean it will have the intend effect. The appropriate public forum to gauge interest around your ACCP is [the Ampleforth Discord].

Your role as the champion is to write the ACCP using the style and format described below, shepherd the discussions in the appropriate forums, and build community consensus around the idea. Following is the process that a successful ACCP will move along:

```
[ WIP ] -> [ PROPOSED ] -> [ APPROVED ] -> [ IMPLEMENTED ]
```

Each status change is requested by the ACCP author and reviewed by the AIP editors. Use a pull request to update the status. Please include a link to where people should continue discussing your ACCP. The AIP editors will process these requests as per the conditions below.

* **Work in progress (WIP)** -- Once the champion has asked the Ampleforth community whether an idea has any chance of support, they will write a draft ACCP as a [pull request].

* **Proposed** If agreeable, AIP editor will assign the ACCP a number (generally the issue or PR number related to the ACCP) and merge your pull request. The AIP editor will not unreasonably deny an ACCP. Proposed ACCPs will be discussed on governance calls and in Discord. If there is a reasonable level of consensus around the change on the governance call the change will be moved to approved. If the change is contentious a vote of token holders may be held to resolve the issue or approval may be delayed until consensus is reached.

* **Approved** -- This ACCP has passed community governance and is now being prioritised.

* **Implemented** -- This ACCP has been implemented and the variable changed on mainnet.

## What belongs in a successful ACCP?

Each ACCP should have the following parts:

- Preamble - RFC 822 style headers containing metadata about the ACCP, including the ACCP number, a short descriptive title (limited to a maximum of 44 characters), and the author details.
- Simple Summary - “If you can’t explain it simply, you don’t understand it well enough.” Provide a simplified and layman-accessible explanation of the ACCP.
- Abstract - a short (~200 word) description of the variable change proposed.
- Motivation (*optional) - The motivation is critical for ACCPs that want to update variables within Ampleforth. It should clearly explain why the existing variable is not incentive aligned. ACCP submissions without sufficient motivation may be rejected outright.
- Copyright Waiver - All ACCPs must be in the public domain. See the bottom of this ACCP for an example copyright waiver.

## ACCP Formats and Templates

ACCPs should be written in [markdown] format.
Image files should be included in a subdirectory of the `assets` folder for that ACCP as follows: `assets/accp-X` (for accp **X**). When linking to an image in the ACCP, use relative links such as `../assets/accp-X/image.png`.

## ACCP Header Preamble

Each ACCP must begin with an [RFC 822](https://www.ietf.org/rfc/rfc822.txt) style header preamble, preceded and followed by three hyphens (`---`). This header is also termed ["front matter" by Jekyll](https://jekyllrb.com/docs/front-matter/). The headers must appear in the following order. Headers marked with "*" are optional and are described below. All other headers are required.

` aip:` <ACCP number> (this is determined by the AIP editor)

` title:` <ACCP title>

` author:` <a list of the author's or authors' name(s) and/or username(s), or name(s) and email(s). Details are below.>

` * discussions-to:` \<a url pointing to the official discussion thread\>

` status:` < WIP | PROPOSED | APPROVED | IMPLEMENTED >

` created:` <date created on>

` * updated:` <comma separated list of dates>

` * requires:` <AIP number(s)>

Headers that permit lists must separate elements with commas.

Headers requiring dates will always do so in the format of ISO 8601 (yyyy-mm-dd).

#### `author` header

The `author` header optionally lists the names, email addresses or usernames of the authors/owners of the ACCP. Those who prefer anonymity may use a username only, or a first name and a username. The format of the author header value must be:

> Random J. User &lt;address@dom.ain&gt;

or

> Random J. User (@username)

if the email address or GitHub username is included, and

> Random J. User

if the email address is not given.

#### `discussions-to` header

While an ACCP is in WIP or Proposed status, a `discussions-to` header will indicate the mailing list or URL where the ACCP is being discussed.

#### `created` header

The `created` header records the date that the ACCP was assigned a number. Both headers should be in yyyy-mm-dd format, e.g. 2001-08-14.

#### `updated` header

The `updated` header records the date(s) when the ACCP was updated with "substantial" changes. This header is only valid for ACCPs of Draft and Active status.

#### `requires` header

ACCPs may have a `requires` header, indicating the ACCP numbers that this ACCP depends on.

## Auxiliary Files

ACCPs may include auxiliary files such as diagrams. Such files must be named ACCP-XXXX-Y.ext, where “XXXX” is the ACCP number, “Y” is a serial number (starting at 1), and “ext” is replaced by the actual file extension (e.g. “png”).

## AIP Editors

The current AIP editors are

` * Ahmed Naguib Aly (@ahnaguib)`

` * Nithin Krishna (@nithinkrishna)`

` * Brandon Iles (@brandoniles)`

## AIP Editor Responsibilities

For each new ACCP that comes in, an editor does the following:

- Read the ACCP to check if it is ready: sound and complete. The ideas must make technical sense, even if they don't seem likely to get to final status.
- The title should accurately describe the content.
- Check the ACCP for language (spelling, grammar, sentence structure, etc.), markup (Github flavored Markdown), code style

If the ACCP isn't ready, the editor will send it back to the author for revision, with specific instructions.

Once the ACCP is ready for the repository, the AIP editor will:

- Assign an ACCP number (generally the PR number or, if preferred by the author, the Issue # if there was discussion in the Issues section of this repository about this ACCP)

- Merge the corresponding pull request

- Send a message back to the ACCP author with the next step.

Many ACCPs are written and maintained by developers with write access to the Ampleforth codebase. The AIP editors monitor ACCP changes, and correct any structure, grammar, spelling, or markup mistakes we see.

The editors don't pass judgment on ACCPs. We merely do the administrative & editorial part.

## History

The ACCP document was derived heavily from the EIP Ethereum  Improvement Proposal document in many places text was simply copied and modified. Any comments about the ACCP document should be directed to the AIP editors. The history of the EIP is quoted below from the EIP document  for context:

* *"This document (EIP) was derived heavily from [Bitcoin's BIP-0001] written by Amir Taaki which in turn was derived from [Python's PEP-0001]. In many places text was simply copied and modified. Although the PEP-0001 text was written by Barry Warsaw, Jeremy Hylton, and David Goodger, they are not responsible for its use..."* *

Oct 12, 2020: ACCP-1 has been drafted and submitted as a PR.


See [the revision history for further details](https://github.com/ampleforth/**), which is also available by clicking on the History button in the top right of the ACCP.

### Bibliography

[the Ampleforth Discord]: https://discord.gg/6Amxhs4
[pull request]: https://github.com/Ampleforth/AIPs/pulls
[markdown]: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet
[Bitcoin's BIP-0001]: https://github.com/bitcoin/bips
[Python's PEP-0001]: https://www.python.org/dev/peps/
[Ampleforth Core Contributors]: https://github.com/orgs/Ampleforth/people

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
