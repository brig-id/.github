# Contributing to brig·id

Thanks for helping build brig·id.

## Before opening a pull request

1. Start from the latest default branch.
2. Keep changes focused.
3. Update documentation when behavior or setup changes.
4. Add or update tests when the repository already includes them.

## Pull requests

- Explain the problem and the chosen approach.
- Link the related issue when relevant.
- Call out follow-up work explicitly.

## Sign your commits off (Developer Certificate of Origin)

Every repository of brig·id is free software, under the licence named in its `LICENSE`
file. A contribution is given under that same licence. There is nothing to sign and no
agreement to send: instead, each commit carries one line saying that you have the right to
give it.

```text
Signed-off-by: Ada Lovelace <ada@example.org>
```

In plain words, that line says: I wrote this change, or I have the right to pass it on
under the repository's licence, and I understand that the record of it, my name and
address included, is public and kept. It is the
[Developer Certificate of Origin](https://developercertificate.org/), version 1.1, quoted
in full below.

- `git commit -s` adds the line, with the name and address of your git configuration
  (`user.name`, `user.email`). The name and the address must be the commit's author's.
- To add it to the commits of a branch you already made:
  `git rebase --signoff origin/main`, then push your branch again.
- A check on every pull request verifies that each commit has a `Signed-off-by` line that
  matches its author. It applies to people outside the brig·id organisation; its members
  and the update bots (Dependabot, Renovate) are exempt.

<details>
<summary>The Developer Certificate of Origin 1.1</summary>

```text
Developer Certificate of Origin
Version 1.1

Copyright (C) 2004, 2006 The Linux Foundation and its contributors.

Everyone is permitted to copy and distribute verbatim copies of this
license document, but changing it is not allowed.


Developer's Certificate of Origin 1.1

By making a contribution to this project, I certify that:

(a) The contribution was created in whole or in part by me and I
    have the right to submit it under the open source license
    indicated in the file; or

(b) The contribution is based upon previous work that, to the best
    of my knowledge, is covered under an appropriate open source
    license and I have the right under that license to submit that
    work with modifications, whether created in whole or in part
    by me, under the same open source license (unless I am
    permitted to submit under a different license), as indicated
    in the file; or

(c) The contribution was provided directly to me by some other
    person who certified (a), (b) or (c) and I have not modified
    it.

(d) I understand and agree that this project and the contribution
    are public and that a record of the contribution (including all
    personal information I submit with it, including my sign-off) is
    maintained indefinitely and may be redistributed consistent with
    this project or the open source license(s) involved.
```

</details>

## The name

What a fork or a host may call itself is in [`TRADEMARKS.md`](TRADEMARKS.md).

## Issues

- Use the provided templates when possible.
- Include reproduction steps for bugs.
- Describe the expected outcome for feature requests.

## Organization bootstrap

Until more repositories exist, `.github` and `.dev` are the reference repositories for shared conventions.
