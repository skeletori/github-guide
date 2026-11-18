# 5. Code Review

[← Back to contents](../README.md)

Code review is how teams share knowledge and catch problems. As a junior, **you should review code too**. It's one of the fastest ways to learn a codebase.

## Receiving feedback

- **Comments are about the code, not about you.** Everyone gets review comments, including senior engineers.
- **Assume good intent.** Text loses tone; a terse comment is usually just a busy person.
- **Reply to every comment.** Fix it, or explain why not. "Good catch, fixed in a1b2c3d" is perfect.
- **Ask when you don't understand.** "Could you say more about why X is preferable here?" is a great question.
- **Disagree with reasons, not feelings.** If a thread goes past 2–3 back-and-forths, move to a quick call.
- **Don't resolve other people's threads** unless your team says that's fine. Let the reviewer confirm.
- **Say thanks.** Reviewing takes time.

## Giving feedback

### What to look for (in rough priority order)

1. **Correctness:** Does it do what it claims? Edge cases, error handling, off-by-ones?
2. **Security:** Input validation, auth checks, secrets, injection risks.
3. **Design:** Does it fit the codebase? Is it in the right place?
4. **Tests:** Are the important paths covered? Would the tests catch a regression?
5. **Readability:** Clear names, reasonable function size, comments where the *why* isn't obvious.
6. **Style:** Ideally handled by linters and formatters, not humans.

### How to comment

- **Be specific and kind.** Explain the *why* and suggest an alternative.
- **Ask questions** rather than issuing commands: "What happens if `user` is null here?"
- **Label the weight** of each comment so the author knows what's blocking:

| Prefix | Meaning |
|---|---|
| `blocking:` / *(no prefix)* | Must change before merge |
| `suggestion:` | I think this is better, but your call |
| `nit:` | Tiny/stylistic, non-blocking |
| `question:` | I want to understand, not necessarily change |
| `praise:` | Something done well, so say so! |

This is loosely based on [Conventional Comments](https://conventionalcomments.org/).

✅ Good:

> `suggestion:` this loop queries the DB once per item. Could we fetch them all with a single `WHERE id IN (...)` to avoid N+1 queries?

❌ Not great:

> This is slow.

### Use GitHub's "suggested changes"

For small fixes, click the ± icon in a review comment to propose exact code. The author can apply it with one click.

````markdown
```suggestion
const MAX_RETRIES = 3;
```
````

### Review outcomes

- **Approve:** good to merge (maybe with nits).
- **Comment:** feedback without a verdict.
- **Request changes:** something must change before merging. Use it sparingly and explain why.

## Reviewing as a junior

You don't have to be an expert to add value:

- Ask questions about anything unclear. If you don't get it, the next reader won't either.
- Check that the PR description matches what the code actually does.
- Pull the branch and run it: `gh pr checkout 123`.
- It's fine to say: "I'm not familiar with this area. Looks reasonable to me, but I'd like someone else to approve too."

## Response time

Unblocking teammates matters. Aim to give a first review within one working day. If you can't, say so.
