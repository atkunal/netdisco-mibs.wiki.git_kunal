## General
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