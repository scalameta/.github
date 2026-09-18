# Agents

Read these before you change anything, in this order:

1. [`CONTRIBUTING.md`](https://github.com/scalameta/.github/blob/main/CONTRIBUTING.md)
   in the shared organization repository. It holds the rules for every change,
   and a section for agents.
2. The contributing file of the repository you are working in. It holds the
   layout and the build, test and format commands.
3. The rest of this file. These rules bind every commit an agent writes.

If you cannot read the first file, stop and tell the author. An agent that
cannot read the files in the shared organization repository may not contribute
to any repository of this organization.

## Find the cause and the cases like it

A bug report shows one input. Before you change any code:

1. Ask why the output is wrong, then why that is so, until you reach the code
   that decides the behaviour, and not the code where the wrong output
   appears. A fix in the second place treats the symptom, and everything else
   that depends on the first keeps the bug.
2. While you investigate, you notice differences: this case is not handled as
   the others are. Such a difference is the lead to follow, not a note to
   move past.
3. Find the other inputs that the same code handles: the other shapes of the
   same construct, and the other constructs on the same path. For each one,
   find out how the code handles it today and which test covers it. One that
   works shows the path the failing one must take.
4. Make the failing input follow the ones that work, in the code they share. A
   fix that adds a branch for one input, where the others share one path, is
   in the wrong place.
5. The test commit pins every input you found: the failing ones as they fail
   today, the working ones as they work. One that has a test already needs no
   new one. The fix commit then shows which inputs it changes.

None of this goes into a commit message or a pull request body. The test
commit shows the cases, and the fix commit shows the cause.

## A bug fix as a proof

A bug fix arrives as a sequence of commits that a reviewer reads as a proof:

1. Preparation. Any refactor or plumbing the fix needs, each in its own
   commit, with no behaviour change. Such a commit may add a parameter that
   changes nothing yet, together with the tests that pin today's output.
2. Tests. One commit adds every test the fix will touch, and a test for every
   input found above that has none. Each test asserts the behaviour as it is
   today. A test that captures the bug is green: it asserts
   the exception thrown today, or the wrong value returned today. A test for a
   case that already works asserts that too. Add a test here even if the fix
   will break its case. The fix commit then shows the flip.
3. The fix. Each fix commit changes production code and every assertion it
   affects, together, so that the diff shows each assertion move from one
   truth to the next.

A test stays the same test across commits. Only its assertions move:

- The title describes the setup, never the outcome: "self-referencing jar",
  not "self-ref reports a cycle".
- The exercised call stays identical. If the behaviour changes from throwing
  to returning, hoist the call into a stable `def load() = ...` and let only
  the asserting lines change.
- The ideal fix diff for a test is only `-assert(...)` and `+assert(...)`.

A test is impossible to write only when no test can run the path today. When
the only answer today is "this does not exist", the test arrives with the
code. However, if the only reason is because some configuration parameter
doesn't exist, then add inert configuration parameters first, along with
tests with different values of these parameters showing how these parameters
make no difference; and the fix commit should flip them.

## Assertions

An assertion states what must hold, never a number you measured. If a design
works only because of today's data, enforce the property or handle the other
case.

## Commit messages

- The subject says the change, in at most 50 characters. Many commits need
  only that.
- Write a body only when the reason is not in the title and not immediately
  obvious from the diff.
  Its first sentence is that reason. It opens on the previous state and turns
  with "Now".
- The reader wrote the surrounding code and already understands the change.
  Write only what that reader cannot read off the diff. Never point at a
  comment inside the diff.
- Omit routine noise/table-stakes: test counts, "formatted", "cross-build ran".
- Wrap the body at 72 characters. Do not break a URL, a path or an identifier.
- A measurement goes in the commit message, never in a code comment. A commit
  is dated, so a number in it stays true.

## Comments

- Write no comment by default. Write one only when the reader must know that
  something matters, and point at the part that does. Never document the
  obvious: a definition whose body says what it does gets no comment.
- A comment is true at the commit it lives in. Do not state what the code will
  mean after a later commit. Changing the comment between commits in the
  same PR means you have described the wrong thing (the "how" rather than
  the "why" or "what").
- Code and comments describe the standing state. They never narrate a change
  and never say who wrote them.
- A comment that spans more than one line is a block comment, `/*` with ` *`
  continuations. `//` is for a comment that fits on one line or code block
  commented out by an IDE (`//` is in column 1).
- An sequence of comments (including scaladoc) unbroken by a blank line
  belongs to the code on the next line, and nothing may come between them.
- Keep every comment within the project's `maxColumn`.

## Plain English

These rules come from Part 1 of [ASD-STE100](https://www.asd-ste100.org/),
Simplified Technical English, and from relevant pages of the
[Google developer documentation style guide](https://developers.google.com/style),
such as:
- [writing for a global audience](https://developers.google.com/style/translation),
- [anthropomorphism](https://developers.google.com/style/anthropomorphism) and
- [lists](https://developers.google.com/style/lists).
They govern every comment, commit message, pull request body and reply.

- One idea per sentence, twenty words at most.
- Name the actor and use the active voice: "mima skips the class", not "the
  class is skipped".
- Write a condition as `if`, never as a relative clause.
- Name the thing: the file, the class, the setting, the version.
- One word, one meaning. Use the term the code uses and repeat it.
- Put "only" directly before the word it limits.
- Use simple tenses.
- No metaphor. Say what happens.
- Negate the verb, never the object: "the list does not include a platform".
- A thing takes the verb for what it is built to do. A method returns, a
  config stores, a build produces. A thing never names, tells, hands or knows.
- Use standard, common engineering verb/terms, do not invent your own.
- Write several items that differ from each other as a list, one per line.
  Introduce it with a complete sentence that ends in a colon. Give every item
  the same shape. Number the items only when the order matters. A single item
  is a sentence, not a list.
