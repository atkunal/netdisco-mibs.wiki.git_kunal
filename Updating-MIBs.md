1. Configure git to help you develop netdisco-mibs:

    * `git config --global core.autocrlf input` (fix MIB line endings)
    * `git config --global push.default current` (push and track new branches)
    * `git config --global core.whitespace cr-at-eol` (make git-diff ignore windows line endings)
    * `git clone https://github.com/netdisco/netdisco-mibs.git` (replace Netdisco's netdisco-mibs if you wish)

1. Unzip your new MIB bundle to e.g. `/tmp/vendorname` (vendorname must exist in netdisco-mibs or be new)
1. Assuming you have a `git clone` of the repo, set your `MIBHOME` environment variable to that location on your computer. `MIBHOME` must not include spaces or wide characters (emoji, etc).
1. `cd $MIBHOME`
1. Create a new branch for your work: `git checkout -b initials-vendorname-description` (your initials, the vendor, a description)
1. If necessary, `mkdir vendorname` (if the vendor is new)
1. Bootstrap net-snmp with custom configuration: `EXTRAS/scripts/setmaxtc`

    * _Run this **every** time. It will only be slow once, when building the apps._

1. Update indexes: `EXTRAS/scripts/mkindex`
1. Prepare the MIBs for import: `EXTRAS/scripts/prepmibs /tmp/vendorname`

    * _The script will rename and organise files to help you._
    * _Items marked "✘" need manually inspecting and will be in the "error" folder._
    * _Items marked "⚠" will be put in the "ignore" folder and skipped._
    * _MIBs belonging to other vendors will be moved to the "other" folder._
    * _MIBs in "other" may be dependencies. If so, run `prepmibs`+`importmibs` on them._
    * _Use `compare <mibfile>` to diff a MIB file against the netdisco-mibs version._
    * _Watch out for new entries that could be bundled RFCs!_

1. Import the MIBs: `EXTRAS/scripts/importmibs /tmp/vendorname`
1. Rebuild indexes to refer to new MIBs: `EXTRAS/scripts/mkindex`
1. Test load new mibs: `EXTRAS/scripts/testload vendorname`

    * _Re-run until there are no errors reported in the output._
    * _Use `compare <mibfile>` to diff a MIB file against the netdisco-mibs version._
    * _Also run this for each "other" vendor that you also imported._

1. Run snmptranslate across all mibs: `EXTRAS/scripts/genxlate`

    * _Re-run until there are no errors reported in the output._

1. Inspect snmptranslate diffs:

    * `git diff EXTRAS/reports/vendorname`
    * `git diff EXTRAS/reports/all` (should look the same as the vendor diff)
    * _Sanity check that new entries are what you were expecting._

1. If necessary, add new vendors to `EXTRAS/contrib/snmp.conf`
1. `git add . && git commit -m "a good comment"`
1. `git push`
1. Return to using your primary net-snmp environment: `exit`

If you are a registered developer, you can merge this branch and [publish a new release](https://github.com/netdisco/netdisco-mibs/wiki/Releasing-MIBs).