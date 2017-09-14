# Some git tips
* On MacOS and Linux: `git config --global core.autocrlf input`
* Always push new tags with commits: `git config --global push.followTags true`
* Short diff summary: `git diff --shortstat`

Check out a Github pull request into a branch using the commands in [this gist](https://gist.github.com/ollyg/9db70a621d0638b491354e39e5b27bf1).

# Old Style
* Having indexes helps with verifying the MIB collection using the old `chk_*` scripts from Max
* Always run `snmptranslate -Tt` on the whole collection before changing anything, and closely inspect the diffs
* Do a lot of intermediate `snmptranslate` sequences as well, to spot errors or incompatible MIB updates

Remove all files in `/var/net-snmp/mib_indexes/`, run the command `snmptranslate -Tt | sed -re 's/ tc=[0-9]+//g'`
and redirect its output to a file, fixing items in STDERR until it's empty.

The STDOUT output is the one to compare between "starting point" and "end point" of changes.

After that `snmptranslate` routine, run the `regen_indexes.pl` script, then the `mkindex` script, then the `chk_*` scripts. Fix issues, rinse, repeat.

So something like...

1. run snmptranslate -Tt and save STDOUT
2. run snmptranslate -Tt inspecting errors (rinse, repeat....)
3. run snmptranslate and compare to (1)
4. run regen indexes to copy system indexes
5. run mkindex to gather indexes
6. run chk_dups
7. run chk_mibs (should be error free?)


