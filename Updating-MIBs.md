# New Style (draft)

1. Set git to translate line endings:

    * `git config --global core.autocrlf input`

1. Assuming you have a `git clone` of the repo, set your `MIBHOME` environment variable to that location.
1. untar your new mib bundle to e.g. `/tmp/vendorname` (vendor name must be used)
1. update indexes: [`netdisco-mibs/extras/scripts/`]`mkindex`
1. prepare the MIBs for import: `prepmibs /tmp/vendorname`

    * _The script will rename and organise files to help you._
    * _Any items marked "✘" need manually inspecting and will be in the "error" folder._
    * _Take a look at items marked "⚠" in the "ignore" folder, just in case._
    * _Watch out for new entries that could be RFCs._
    * _MIBs belonging to other vendors will be moved to "other" folder._
    * _The `compare` script can be run on a MIB file to diff it with the netdisco-mibs version._
    * _Run `prepmibs` (& import...) on each folder in "other" if those dependencies are required._

1. import the MIBs: `importmibs /tmp/vendorname`
1. rebuild indexes to refer to new MIBs: `mkindex`
1. bootstrap net-snmp with sufficient MAXTC: `setmaxtc`

    * _Run this every time. It will only be slow the first time, building the apps._

1. test load new mibs: `testload vendorname`

    * _Re-run until there are no errors reported in the output._
    * _The `compare` script can be run on a MIB file to diff it with the netdisco-mibs version._
    * _Remember to run this for each "other" vendor that you also imported._

1. run snmptranslate across all mibs: `genxlate`

    * _Re-run until there are no errors reported in the output._

1. inspect snmptranslate diffs: `git diff netdisco-mibs/extras/reports/all`

    * _Sanity check that new entries are what you were expecting._

1. add new vendors to `extras/contrib/snmp.conf`
1. done! `git commit ...`

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


