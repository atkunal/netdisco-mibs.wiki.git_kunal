When a pull request is made, we first need to check that the user followed the new process for [updating MIBs](Updating-MIBs).

Add the following lines to your user's `.gitconfig` (in your home area):

```
[alias]
  pr = "!sh -c \"git checkout -b pr/$1 && curl -sL $(git config --get remote.origin.url | sed -e 's|:|/|' -e 's|^git@|https://|' -e 's|\\.git$|/pull/$1.patch|') | git am --whitespace=nowarn\" -"
  pr-clean = "!git checkout - ; git for-each-ref refs/heads/pr/* --format=\"%(refname)\" | while read ref ; do branch=${ref#refs/heads/} ; git branch -D $branch ; done"
```

1. make sure you are at a clean HEAD state (no edited files which are not committed)
1. check out the pull request (e.g. number 14) into a new branch:

    `git pr 14`

1. inspect the snmptranslate reports to make sure that only the intended vendor is updated (for example some vendors may impact others' namespaces)

    * `EXTRAS/scripts/mkindex`
    * `EXTRAS/scripts/prepmibs vendorname`
    * `EXTRAS/scripts/testload vendorname`
    * `EXTRAS/scripts/genxlate`
    * `git diff master...`

1. if you're happy, go to github and perform an automatic squash merge in the web interface
1. delete the local PR branch:

    `git pr-clean`

However...
1. if you need to make changes, commit them to the local branch
1. fetch the pull request's origin branch (the USERNAME and BRANCHNAME will be in the PR page on github):

    `git fetch https://github.com/USERNAME/netdisco-mibs.git BRANCHNAME`

1. then force push changes from your local branch back to the pull request at github (NN is the PR number):

    `git push --no-follow-tags https://github.com/USERNAME/netdisco-mibs.git +pr/NN:BRANCHNAME`

1. go to github and perform an automatic squash merge in the web interface
1. delete the local PR branch:

    `git pr-clean`