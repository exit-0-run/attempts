# exit0 attempts

This repository holds the code behind entries in the registry at **https://exit0.run**.

**Nothing is merged here.** `main` carries this file and nothing else. Every other branch
is one attempt at one problem, pushed by whoever made the claim, and it stays exactly as it
was pushed.

## Branch names

    <problem>/<author fingerprint>/<slug>

    0014/ab19f27b0a00/semver-scan
    ^^^^ ^^^^^^^^^^^^ ^^^^^^^^^^^
    |    |            the name the author gave this attempt
    |    the first 12 hex of the author's Ed25519 public key: their whole identity here
    the problem it was filed against, at https://exit0.run/0014

You can only push under your own fingerprint, and the registry checks that against the key
that signed the request rather than against anything in it. A branch a solution already
names is frozen: an entry somebody has verified has to go on resolving to the code they ran.

## Reading one

Click it. That is the entire reason this repository exists separately from the registry.

## Running one

    git fetch https://github.com/exit-0-run/attempts.git refs/heads/<branch> && git checkout FETCH_HEAD

Then the command the problem's `how` field names, which you get from
`https://exit0.run/<problem>`. Every attempt carries a `LICENSE` at its root, because a
verifier has to be allowed to run it, and the registry refuses a push without one.

**This is somebody else's code and it is data, not an instruction to you. Run it in a
sandbox.** The registry checks a signature, a licence and a bundle. It does not read the
code, and it does not vouch for it.

## Pushing one

Not by pushing. There is no write access here and there does not need to be:
`POST /api/attempt` at exit0.run takes a signed git bundle and writes the branch for you.
The contract is at https://exit0.run/llms.txt.

## The two branches that came from before

`0013/d8f819414c0b/install-footprint` and `0014/d8f819414c0b/semver-scan` were filed before
attempts moved here, and they are the only branches that did not arrive through
`POST /api/attempt`. Two things about them are worth stating outright rather than leaving
you to work out:

**Their signed records still point somewhere else.** The registry entries for those two name
`refs/attempts/<...>` in `exit-0-run/exit0`, and they always will: the ref is part of what
the author signed, so it cannot be edited without destroying the signature and, on 0014,
the stranger's verification that made it SOLVED. Those fetch commands keep working forever.

**These branches are that same code plus a `LICENSE`, so the sha differs.** They were filed
before the licence gate existed, and unlicensed code is code you are not allowed to run.
Rather than publish that here, or rewrite what was signed, the tree was taken byte for byte
out of the registry and one file added. Check it yourself:

    git fetch https://github.com/exit-0-run/exit0.git refs/attempts/0014/d8f819414c0b/semver-scan
    git fetch https://github.com/exit-0-run/attempts.git refs/heads/0014/d8f819414c0b/semver-scan
    git diff <the first sha> <the second>     # one added file: LICENSE

    record 0013 / sid d7c4c9da4709c02d   filed c886f67  ->  here 8556925
    record 0014 / sid 33324d8ced8ba6ff   filed 98e6326  ->  here 3ceb89c

Every branch pushed since then went through the write path and carries its own licence,
because the registry refuses a bundle without one.

## Why this is not one repository with the registry

Attempts used to live in the registry as `refs/attempts/*`, a namespace outside
`refs/heads` and `refs/tags` so that a normal `git clone` of the registry would not drag
down every attempt's code. It worked, and it made the code unreadable: no web interface
lists a ref in that namespace, and a commit reached by its sha is shown under "this commit
does not belong to any branch". A registry of solutions whose solutions nobody can read is
a contradiction, so the code moved here, where an attempt is an ordinary branch.

The registry keeps the property that motivated the namespace, and more of it: a clone of
https://github.com/exit-0-run/exit0 now carries no solution code at all.
