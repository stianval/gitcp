# Introduction

This repository demonstrates that copying files under git can be done in two
different ways, namely the conventional way, and a special way. The rationale
of the special way is that it will more naturally preserve the history for the
file that was copied for the copy as well. That is, `git log --follow` and `git
blame`, when used on the copy, will display history for the original file that
happened prior to the copy.

# Background

Before we consider copying in git, let's first consider another situation,
namely simply moving a file. In this case git will be able to detect that the
file was moved in this commit by noticing there is a file that was created and
a similar file that was removed. The important thing is that git only need to
consider files that are involved in the commit itself to deduce what happened.

Contrast this with what happens when a file is copied. The copy is added to the
history, but git will have to consider files outside the commit to determine
where it was copied from. By default, `git log --follow` and `git blame` will
only trace back and make move deductions based on files that exist in the same
commit.

There are flags that will extend this trace to involve files in the repo
outside a given commit as well, but this has three drawbacks
1. The algorithm becomes more expensive
2. Such flags can be hard to remember/discover, mostly only experts would ever
   reach for them
3. Many tools that uses git for log and blame (e.g. editor plugins, GitHub)
   does not expose these flags, or or add another layer of difficulty of
   discovery

If you're the expert, you can save yourself this way. But also, if you're the
expert, I plan to describe a way for you to save everyone, by more carefully
craft copy-heavy commit so `git log --follow` and `git blame` can still work
with no extra flags on all copies of the files.

For those anyway still curious what those flags are, for blame one can do `git
blame -C -C` on the relevant file (note:  two `-C`).

# Procedure

If you find yourself in need to copy one or more files in a repo, you can
follow the following procedure:
1. Copy the files you want to copy to the desired location.
2. In addition rename the files you copied, or move them to a separate
   location.
3. Commit all changes.
4. Move the renamed or moved files back to their original name or location.
5. Commit these changes.

After this you will find that `git log --follow` and `git blame` can still trace history
preceding the commit created in step 2 for all copies of the file.

# Demo

To demonstrate that the above procedure works, I have created a file in this
repo, named `to_double_move.md`. This file was first edited trough a few
commits, before it was copied according to the above procedure. The additional
copy exists in `copies/to_double_move.md`. If inspecting either of these files
with `git log --follow` or `git blame`, one will find that these tools can show
commits that happened before the copy took place.

As a reference, I also created a file named `to_copy.md`. This file was
similarly edited through a few commits, then copied normally to
`copies/to_copy.md`, and then the copy was added and commited in git. If trying
to inspect `copies/to_copy.md` with `git log --follow` or `git blame`, one will
find that the first reported commit is the copy commit that created the file.
