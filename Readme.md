### Setup your configuration

The setup wizard interactively configures your flow and neos forks and will also create the forks for you if needed.
It further renames the original remotes to "upstream" and adds your fork as remote with name "origin".

	./flow github:setup


### Transfer a Gerrit change to Github

The command

	./flow github:applyGerritChange <gerritPatchId>

transfers a Gerrit patch to a your development collection in a new branch. In detail it does the following steps:

1. Pull the change as patch from Gerrit
2. Create a new branch for that patch
3. Applies the patch
4. Converts the patch to PSR-2 and adjust license header
5. Rebases patch onto latest master

If there are conflicts, they have to be resolved manually.

Commit the modified files with the provided commit message.

Then use that branch to create a new pull request from.

#### Manually transfer a Gerrit change to Github

These are the steps you have to do to manually move a Gerrit patch to Github. The example commands move a patch for the package TYPO3.TYPO3CR. If you have changes that are stacked on each other, repeat step 6 until all needed changes have been applied, then continue.

1. Go to your change on https://review.typo3.org
2. If you haven't done yet, fork the Neos Development Collection repository and clone the fork on your machine
3. Bring your local fork repository and code up to date with the upstream repository
4. Add a new branch, for example "change-xx-yyyyy-z" or "neos-12345"
5. Navigate to the package directory (e.g. Packages/Neos/TYPO3.TYPO3CR/)
6. Patch your code using `git am` after having copied the "fomat patch" git command. Example:

   `git fetch http://review.typo3.org/Packages/TYPO3.TYPO3CR refs/changes/xx/yyyyy/z && git format-patch -1 -k --stdout FETCH_HEAD | git am -k --directory TYPO3.TYPO3CR`

   Mind the added `-k` for the `format-patch` and `am` git commands, it makes sure tags like `[TASK]` are kept.

   If you are patching a repository that is not a "development collection", you can leave out `--directory`.

7. Check the result (you may need to amend the commit to fix the [<TAG>] used in the subject line if you left
   out the `-k` option)
8. Push the changes to origin/branchName
9. Go to Github and open a pull request
9. Abandon the change on Gerrit.

Hint: If you are using SourceTree, steps 8 and 9 can be done by using the "Create Pull Request" item in
the "Repository" menu.

#### Other resources

- [Hub](https://hub.github.com/) is a command-line wrapper for git that makes you better at GitHub.
- [How to undo (almost) anything with Git](https://github.com/blog/2019-how-to-undo-almost-anything-with-git)