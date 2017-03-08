# New Style (draft)

1. update indexes: `mkindex`
1. untar your new mib bundle to e.g. `/tmp/vendorname`
1. import the new mibs: `importmibs /tmp/vendorname`

 _The script will report various issues, such as:_
 * _files that are not MIBs (and will not be imported)_
 * _files that contain MIBs owned by another vendor_
 * _MIBs that are the same or older releases as existing ones_

 _You'll need to fix issues marked with "✘" before continuing._

1. update indexes again: `mkindex`
1. bootstrap net-snmp with sufficient MAXTC: `setmaxtc`

 _Run this every time. It will be slow on the first run, while building the apps._
1. test load new mibs: `testmibs vendorname`

 _Re-run until there are no errors reported in the output._
1. run snmptranslate across all mibs: `genxlate`

 _Re-run until there are no errors reported in the output._
1. inspect snmptranslate diffs: `git diff netdisco-mibs/extras/reports/all`

 _Sanity check that new entries are what you were expecting._
1. done! `git commit ...`

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


