1. Follow the [Updating MIBs](https://github.com/netdisco/netdisco-mibs/wiki/Updating-MIBs) docs.
1. Configure git to help you release:

    * `git config --global push.followTags true` (push new tags together with commits)

1. Edit `README` and increment the version (e.g. `X.Y`).
1. Commit this change:

    * `git add README`
    * `git commit -m 'release X.Y'`

1. Tag it:

    * `git tag -a X.Y -m 'version X.Y'`

1. Push to git:

    * `git push` (may need `--follow-tags` if you have an old version of git)

1. Visit https://github.com/netdisco/netdisco-mibs/releases/new
1. Select your new tag (X.Y) from the dropdown list.
1. Enter "netdisco-mibs-X.Y" for the Release Title
1. Click the green "Publish Release" button.

Done!