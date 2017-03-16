## New Style
> _These are listed roughly in the order that you'd run them in a standard workflow._

> _Run everything from a git clone, setting `MIBHOME` environment variable to that location._

### mkindex
* Triggers net-snmp to build index files for netdisco-mibs (in a temporary location)
* Caches those indexes in a portable way (removing your system's directories)
* Writes `mib_index2.txt` file containing all vendor MIBs and their files
* _Changes files in the git repo_

### prepmibs
* Organises files in new_mibs_dir (presumably a messy vendor bundle) in preparation for import
* Renames any known MIB file to be the same as its equivalent in netdisco-mibs 
* Prints a friendly report telling you about all actions taken:
  * Files that don't look like MIBs (to net-snmp) are moved to the "ignore" subfolder
  * Files that seem to have issues are moved to the "error" subfolder
  * MIBs belonging to other vendors are moved to "other/vendorname" subfolder(s)
  * MIBs older than their equivalent in netdisco-mibs are moved to the "older" subfolder
  * MIBs the same age as in netdisco-mibs are moved to the "same" subfolder

### importmibs
* Copies files from new_mibs_dir to the vendor's folder in netdisco-mibs
* Assumes that prepmibs has been run, so only new and newer MIBs are in new_mibs_dir
* Refuses to run if the "error" subfolder has anything in it
* _Changes files in the git repo_

### setmaxtc
* Builds the net-snmp applications with an amended `MAXTC` so that all MIBs can be loaded at once
* Spawns your shell with `PATH` adjusted to use these applications (`exit` to quit the shell)

### testload
* Run `snmptranslate` on each MIB in a vendor's folder (individually), report errors

### genxlate
* Run `snmptranslate` on all MIBs in netdisco-mibs (at once), report errors
* A tree form of the loaded MIBs and their content (`-Tt`) is saved to `MIBHOME/extras/reports/all`
* _Changes files in the git repo_

### compare
* Given any MIB file, runs a diff against the equivalent MIB in netdisco-mibs

## Old Style (general)
### chk_dups
* Reports if any two files are defining the same MIB. Shows a diff and LAST-UPDATED, if they are
* Needs a mib_index.txt file containing .index from all directories in the bundle

### chk_mibs
* runs snmptranslate on every MIB file listed in a .index and dies if there are errors
* Needs a .index file for the directory

### chk_mibs_all
* Runs chk_mibs for the directories in cwd

### mkindex
* Runs snmptranslate with `-M$dir` to prompt it to regenerate the .index file for `$dir`
* Builds a mib_index.txt from the sorted content of all the .index files

### regen_indexes
* Copies .index files from the net-snmp PERSISTENT_DIR system cache back into local directories
* Strips the DIR line that is added since net-snmp moved from dotfiles to persistent cache

## Discovery
### snmpwalkmib
* Dump the contents of a MIB on a device
* Can also run in a dumb snmpwalk mode from the root(s) in the MIB

### walk_all
* Runs snmpwalkmib on all MIBs mentioned in a mib_index.txt or .index file

## Utilities
### diff_dir
* Compares two directories and makes sure one contains the newest (or only) versions of all files 

### to_locase
* Renames a file name to lowercase (I suppose if you don't have `rename`)