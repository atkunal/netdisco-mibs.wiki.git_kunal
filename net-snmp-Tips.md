# Configuration paths in the build

`snmpd -f -Lo -Dread_config -H 2>&1 | grep "config path" | head -1`

For example (on MacOS):

`read_config:path:  config path used for snmpd:/etc/snmp:/usr/share/snmp:/usr/lib/snmp:/Users/oliver/.snmp (persistent path:/var/db/net-snmp)`

Or even better, use `net-snmp-config`:

    --configure-options   display original configure arguments
    --prefix              display the installation prefix
    --snmpd-module-list   display the modules compiled into the agent
    --default-mibs        display default list of MIBs
    --default-mibdirs     display default list of MIB directories
    --snmpconfpath        display default SNMPCONFPATH
    --persistent-directory display default persistent directory
    --perlprog            display path to perl for the perl modules

# Interesting environment variables

* SNMPCONFPATH
* SNMP_PERSISTENT_DIR
* MIBDIRS
* MIB