# Contributing code

This guide sets the default rules for a change to repositories in this
organization. Each repository has its own contributing file with the
layout and the build, test and format commands. Read that file first,
it can possibly override or customize some of these rules. This guide
covers what a change must look like before a maintainer reads it. An agent
must also follow the shared
[`AGENTS.md`](https://github.com/scalameta/.github/blob/main/AGENTS.md), and a
human contributor is welcome to.

## Before you push

- Every commit in your branch must be able to land on `main` as it is. The
  maintainer picks rebase, squash or merge per pull request, and you do not
  need to know which.
- CI on a pull request from a new contributor runs only after a maintainer
  approves the run. It would be prudent to validate the changes locally
  before pushing, since it will otherwise lead to review delays and
  additional iterations.

## Commits

- One concern per commit.
- Never mix a functional change with a cosmetic one. A commit either changes
  behaviour or it does not. Formatting, renames and moves go in their own
  commit, and the subject says so.
- Change only what you must. Do not rename when the existing name still fits.
  Do not reflow or reformat a line you are not changing. The ideal review diff
  is exactly the lines of the change.
- If a commit would have to change unrelated lines as well, change those lines
  first, in their own commit, so that the later diff shows only the idea.
- A correction to an earlier commit of the branch goes into that commit. A
  pushed branch has no `fixup!` commits.
- Every commit compiles, passes the whole test suite and is formatted. No
  commit leaves an assertion failing.
- A fix removes the cause, and it covers the cases like the one reported.

## Formatting

Format every commit with the repository's own formatter (likely discoverable
from its .github/ci.yml file). CI checks formatting, so a line the formatter
touches is a line you touched. An unformatted middle commit conflicts against
the formatted ones on a rebase.

## The pull request

- The body says what the change is, why, and how far it reaches. The diff is
  a link. Never narrate it.
- GitHub keeps every newline inside a paragraph, so write one line per
  paragraph.
- A review round is a rewritten branch, force-pushed to the same branch.
  GitHub shows a compare link from the old patch to the new one. Do not add
  `fixup!` commits, and do not open a replacement pull request.
- Answer every review comment before you push the rewritten branch, and
  submit the answers as one review. After a force-push, GitHub does not accept
  a reply on a thread whose line was rewritten.

## Contributing with agents

- Your agent must be able to read the files in the `scalameta/.github`
  GitHub repository. An agent that cannot do it may not contribute to a
  repository of this organization.
- Your agent must read `AGENTS.md` both in the repository containing the
  code and this organization's shared
  [`AGENTS.md`](https://github.com/scalameta/.github/blob/main/AGENTS.md).
  The rules there bind every commit an agent writes, whether the attribution
  says so or not.
- You, the human author, answer for every line. A reviewer may ask about any
  line, and you must be able to explain it. Plenty of tests make that easier,
  so review both the code an agent wrote and the scope and extent of tests.
- A commit message or a pull request body may carry one agent attribution,
  as a trailer such as `Co-authored-by: ...`. Never more than one, and never
  in the code.
- A reply that an agent posts on your behalf says so. It ends with one
  signature in italics, on its own line: `*- <Agent> (on behalf of <you>)*`
  when the words are the agent's, and `*- <Agent> (paraphrasing <you>)*` when
  the words are yours. One signature per reply.

