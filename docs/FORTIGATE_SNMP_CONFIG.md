# Fortigate SNMP Configuration Example

## SNMP v2c Configuration (Recommended for most deployments)

### On FortiGate Device

```
config system snmp sysinfo
    set status enable
    set description "FortiGate-60E"  # Device model/description
    set location "Data Center 1"
    set contact "admin@example.com"
end

config system snmp community
    edit 1
        set name "public"              # SNMP community string
        set status enable
        set events default             # Standard trap events
        set trap-v2c-status enable     # Enable SNMP v2c traps
    next
end

config system snmp community-trap
    edit 1
        set status enable
        set trap-v2c-lport 162
    next
end
```

### In SMON Configuration (config.json)

```json
{
  "snmpDevices": [
    {
      "id": "fortigate-01",
      "name": "FortiGate-60E",
      "host": "192.168.1.100",
      "community": "public",
      "version": "2c",
      "timeout": 5000,
      "retries": 2,
      "pollingInterval": 60000,
      "selectedInterfaces": [
        { "index": 1, "name": "port1" },
        { "index": 2, "name": "port2" },
        { "index": 3, "name": "port3" },
        { "index": 10, "name": "internal" }
      ]
    }
  ]
}
```

## SNMP v3 Configuration (For higher security)

### On FortiGate Device (FortiOS 6.4+)

```
config system snmp user
    edit "monitoring"
        set auth-proto sha
        set auth-pwd ENC <encrypted-password>
        set priv-proto aes
        set priv-pwd ENC <encrypted-password>
        set events default
    next
end

config system snmp community
    # Can keep v2c for specific sources if needed
    edit 1
        set name "public"
        set status enable
        set hosts
            edit 1
                set ip-address 192.168.1.50  # SMON server IP
            next
        end
    next
end
```

### In SMON Configuration (config.json)

```json
{
  "snmpDevices": [
    {
      "id": "fortigate-secure",
      "name": "FortiGate-60E Secure",
      "host": "192.168.1.100",
      "version": "3",
      "username": "monitoring",
      "authProtocol": "sha",
      "authPassword": "your-password",
      "privProtocol": "aes",
      "privPassword": "your-password",
      "timeout": 5000,
      "retries": 2,
      "pollingInterval": 60000,
      "selectedInterfaces": [
        { "index": 1, "name": "port1" },
        { "index": 2, "name": "port2" }
      ]
    }
  ]
}
```

## Verify SNMP Configuration

### From Monitoring Server (Linux)

```bash
# Test SNMP v2c connectivity
snmpwalk -v2c -c public 192.168.1.100 1.3.6.1.2.1.1.1.0

# Test Fortigate-specific CPU OID
snmpget -v2c -c public 192.168.1.100 1.3.6.1.4.1.12356.101.4.1.3.0

# Test memory usage
snmpget -v2c -c public 192.168.1.100 1.3.6.1.4.1.12356.101.4.1.4.0

# List all interfaces
snmpwalk -v2c -c public 192.168.1.100 1.3.6.1.2.1.2.2.1.2

# Get interface RX traffic
snmpget -v2c -c public 192.168.1.100 1.3.6.1.2.1.2.2.1.10.1

# Get interface TX traffic
snmpget -v2c -c public 192.168.1.100 1.3.6.1.2.1.2.2.1.16.1
```

Expected output example:
```
SNMPv2-MIB::sysDescr.0 = STRING: FortiGate-60E v6.4.5, build 0000, 202301301234
.1.3.6.1.4.1.12356.101.4.1.3.0 = INTEGER: 42  # CPU usage: 42%
.1.3.6.1.4.1.12356.101.4.1.4.0 = INTEGER: 68  # Memory: 68%
```

## HA (High Availability) Monitoring

For Fortigate devices in HA mode:

```
config system snmp community
    edit 1
        set name "public"
        set hosts
            edit 1
                set ip-address 192.168.1.50  # Virtual IP (HA cluster IP)
            next
        end
    next
end
```

SMON will automatically detect HA status via:
- `1.3.6.1.4.1.12356.101.13.2.1.0` - HA status
- `1.3.6.1.4.1.12356.101.13.2.1.2.0` - Master serial
- `1.3.6.1.4.1.12356.101.13.2.1.3.0` - Slave serial

## Common Interface Indices

| Index | Interface | Type |
|-------|-----------|------|
| 1 | port1 | Physical |
| 2 | port2 | Physical |
| 3 | port3 | Physical |
| ... | port... | Physical |
| 10 | internal | Internal Switch |
| 11 | aggregate | Aggregated Link |
| 12 | vlan100 | VLAN |
| 13 | vlan200 | VLAN |

## Firewall Rules (if applicable)

Allow SNMP traffic from monitoring server to FortiGate:

```
config firewall address
    edit "Monitor-Server"
        set subnet 192.168.1.50 255.255.255.255
    next
end

config firewall service custom
    edit "SNMP"
        set tcp-portrange 161
        set udp-portrange 161
    next
end

config firewall policy
    edit 1
        set srcintf "wan1"
        set dstintf "internal"
        set srcaddr "Monitor-Server"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "SNMP"
        set logtraffic all
    next
end
```

## Troubleshooting

### SNMP Not Responding

1. Verify SNMP is enabled:
```
diagnose debug sysinfo  # Check if SNMP service is running
```

2. Check firewall rules:
```
diagnose firewall iprule list  # Verify rules allow SNMP traffic
```

3. Test from CLI:
```
diagnose test snmp 192.168.1.50  # Test SNMP connectivity
```

### Wrong Community String

```
execute snmp status  # Show current SNMP configuration
```

### Interface Not Appearing

Some virtual interfaces may not have counters. Use:
```bash
snmpwalk -v2c -c public 192.168.1.100 1.3.6.1.2.1.2.2
```

And select only interfaces with ifInOctets > 0.

## Performance Tips

1. **Polling Interval**: Use 60-120 seconds for optimal balance
2. **Timeout**: 5000ms is sufficient for most FortiGate models
3. **Concurrent Polls**: Limit to 5-10 devices
4. **Interface Selection**: Monitor only needed interfaces (reduces query time)
5. **Log Level**: Set to "warning" or "error" in production

## Security Best Practices

1. Use SNMP v3 with authentication and privacy
2. Restrict SNMP access to specific monitoring IPs
3. Use strong community strings (if using v2c)
4. Disable SNMP traps if not needed
5. Monitor SNMP query logs for anomalies
6. Rotate SNMP credentials regularly

## FortiOS Version Compatibility

- **FortiOS 6.4+**: Full support for all OIDs listed
- **FortiOS 7.0+**: Recommended for best stability
- **FortiOS 7.2+**: Latest version with all MIB enhancements
- **Older versions**: Standard MIB-II always works, Fortigate-specific OIDs may not be available
