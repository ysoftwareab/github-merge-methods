# github merge methods

Visualizing the implementation of
[Github's merge methods](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/about-merge-methods-on-github).

* merge
* squash (and merge with fast forward)
* rebase (and merge with fast forward)

## Results

* merge
  * graph: [result](./merge-graph.txt)
    , [pr](./merge-pr-graph.txt)
  * log [result](https://github.com/ysoftwareab/github-merge-methods/commits/merge)
    , [pr](https://github.com/ysoftwareab/github-merge-methods/commits/merge-pr)
* mergeff (mergeff-pr on top of mergeff)
  * graph: [result](./mergeff-graph.txt)
    , [pr](./mergeff-pr-graph.txt)
  * log [result](https://github.com/ysoftwareab/github-merge-methods/commits/mergeff)
    , [pr](https://github.com/ysoftwareab/github-merge-methods/commits/mergeff-pr)
* squash
  * graph: [result](./squash-graph.txt)
    , [pr](./squash-pr-graph.txt)
  * log [result](https://github.com/ysoftwareab/github-merge-methods/commits/squash)
    , [pr](https://github.com/ysoftwareab/github-merge-methods/commits/squash-pr)
* squashff (squashff-pr on top of squashff)
  * graph: [result](./squashff-graph.txt)
    , [pr](./squashff-pr-graph.txt)
  * log [result](https://github.com/ysoftwareab/github-merge-methods/commits/squashff)
    , [pr](https://github.com/ysoftwareab/github-merge-methods/commits/squashff-pr)
* rebase
  * graph: [result](./rebase-graph.txt)
    , [pr](./rebase-pr-graph.txt)
  * log [result](https://github.com/ysoftwareab/github-merge-methods/commits/rebase)
    , [pr](https://github.com/ysoftwareab/github-merge-methods/commits/rebase-pr)
* rebaseff (rebaseff-pr on top of rebaseff)
  * graph: [result](./rebaseff-graph.txt)
    , [pr](./rebaseff-pr-graph.txt)
  * log [result](https://github.com/ysoftwareab/github-merge-methods/commits/rebaseff)
    , [pr](https://github.com/ysoftwareab/github-merge-methods/commits/rebaseff-pr)

## Observations and reflections

* more often than not, squash and rebase are preferred for a simple/r history,
  but the same effect can be achieved by
  
  * squash-like: `git log --first-parent` which hides the PR commits and only keeps the "Merge pull request #X from SOURCE" commit
  * rebase-like: `git log --no-merges` which hides the "Merge pull request #X from SOURCE" commit and only keeps the PR commits

* rebase will mean there's no merge commit and thus no way to logically and visually group related-commits.
  It will be impossible to know the context of the changes.

* rebase will mean there's no merge commit and thus no way to revert the entire group of related-commits.
  It will be impossible to undo a PR.

* rebase will mean there's no merge commit and thus no reference to the PR number.
  It will be impossible to know the context of the changes.
  
* rebase will mean you lose the GPG signature (squash will retain it).
  It will be impossible to verify source.

* rebase will mean `git log` becomes the same as `git log --no-merges`, and that you lose the ability to `git log --first-parent`.
  It will be impossible to hide "noise".

* squash will mean everything is "noise".
  More often that not, a PR will contain "noise" (e.g. lint, whitespace, typo) fixes, along the core changeset.
  While many will complain about many "noisy" commits, you can distinguish between signal and noise.
  Squashing signal and noise though, gives you noise.
  If you use `git revert` or `git bisect`, then don't squash.
  If you don't `git revert`, then start.
  If you don't `git bisect`, then start.

* squash is not simple at all.
  See [nodejs](https://github.com/nodejs/node/blob/913c365db66c7a0d40e72a463da4a2f3147f0c26/COLLABORATOR_GUIDE.md#landing-pull-requests) requirements for using the squash merge.
  
* more here by @myst729 , including merge-conflicts https://myst729.github.io/posts/2019/on-merging-pull-requests/

* ultimately the merging strategy should NOT be a per-PR choice, but a per-repo choice,
  and it should be "rebase WITHOUT fast-forward preserving GPG signature", in other words a local `git fetch && git rebase origin/master`.


## Conclusion

The beauty of a git history does NOT come from a visual linear history.

The beauty of a git history comes from

* being able to read it: ideally one "merge bubble" at a time, meaning rebase before you merge WITHOUT fast-forward
* being able to use it: `git blame` a line, `git revert` a small commit, `git bisect` down to a small commit

And in the light of the above,

> the merge strategy is the only one worry free, though not ideal either
