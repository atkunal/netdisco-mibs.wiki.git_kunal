# Configuration paths in the build

`snmpd -f -Lo -Dread_config -H 2>&1 | grep "config path" | head -1`

For example (on MacOS):

`read_config:path:  config path used for snmpd:/etc/snmp:/usr/share/snmp:/usr/lib/snmp:/Users/oliver/.snmp (persistent path:/var/db/net-snmp)`

# Interesting environment variables

* SNMPCONFPATH
* SNMP_PERSISTENT_DIR
* MIBDIRS
* MIB