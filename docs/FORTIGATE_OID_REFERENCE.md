# Fortigate SNMP OID Reference

## Complete OID Mapping

### System Information (FORTINET-FORTIGATE-MIB)

```
OID Hierarchy: iso.org.dod.internet.private.enterprises.fortinet
Base OID: .1.3.6.1.4.1.12356

Module: FORTINET-FORTIGATE-MIB
    |
    +-- fg (1.3.6.1.4.1.12356.1)
        |
        +-- fgMgmt (2)
        |   +-- fgMgmtMib (1)
        |       +-- fgSysInfo (101)
        |           +-- fgSysVersion (4)
        |               +-- fgSysVersionMajor (1.0)
        |               +-- fgSysVersionMinor (2.0)
        |               +-- fgSysCpuUsage (3.0)        ← CPU Usage %
        |               +-- fgSysMemUsage (4.0)        ← Memory Usage %
        |               +-- fgSysUptime (1.0)          ← System Uptime
        |               +-- fgSysSerialNumber (2.0)    ← Serial Number
        |               +-- fgSysVdCount (5.0)         ← Virtual Domain Count
```

## Core System OIDs (Status: Implemented)

| Name | OID | Type | Access | Description |
|------|-----|------|--------|-------------|
| fgSysUptime | `1.3.6.1.4.1.12356.101.4.1.1.0` | TimeTicks | RO | System uptime in hundredths of a second |
| fgSysSerialNumber | `1.3.6.1.4.1.12356.101.4.1.2.0` | String | RO | Device serial number |
| fgSysCpuUsage | `1.3.6.1.4.1.12356.101.4.1.3.0` | Integer | RO | CPU utilization percentage (0-100) |
| fgSysMemUsage | `1.3.6.1.4.1.12356.101.4.1.4.0` | Integer | RO | Memory utilization percentage (0-100) |
| fgSysVdCount | `1.3.6.1.4.1.12356.101.4.1.5.0` | Integer | RO | Number of virtual domains |
| fgSysVersion | `1.3.6.1.4.1.12356.101.4.1.6.0` | String | RO | System version string |
| fgSysFlashUsed | `1.3.6.1.4.1.12356.101.4.1.8.0` | Integer | RO | Flash memory used (MB) |

## High Availability (HA) OIDs

| Name | OID | Type | Access | Description |
|------|-----|------|--------|-------------|
| fgHaStatus | `1.3.6.1.4.1.12356.101.13.2.1.0` | Integer | RO | HA status (0=disabled, 1=enabled) |
| fgHaMasterSerial | `1.3.6.1.4.1.12356.101.13.2.1.2.0` | String | RO | Master unit serial number |
| fgHaSlaveSerial | `1.3.6.1.4.1.12356.101.13.2.1.3.0` | String | RO | Slave unit serial number |
| fgHaMode | `1.3.6.1.4.1.12356.101.13.2.1.1.0` | String | RO | HA mode (active-passive/active-active) |
| fgHaSyncStatus | `1.3.6.1.4.1.12356.101.13.2.1.4.0` | Integer | RO | HA sync status |
| fgHaPrimaryStatus | `1.3.6.1.4.1.12356.101.13.2.1.5.0` | String | RO | Primary unit status |

## Interface Statistics OIDs (Standard MIB-II)

All Fortigate models support standard IF-MIB interface statistics:

### 32-bit Counters (Legacy - for older interfaces)

```
Base: .1.3.6.1.2.1.2.2.1.[metric].[ifIndex]

Example for port1 (ifIndex=1):
  ifDescr:      1.3.6.1.2.1.2.2.1.2.1          → "port1"
  ifInOctets:   1.3.6.1.2.1.2.2.1.10.1         → RX bytes
  ifOutOctets:  1.3.6.1.2.1.2.2.1.16.1         → TX bytes
  ifInUcastPkts: 1.3.6.1.2.1.2.2.1.11.1        → RX packets
  ifOutUcastPkts: 1.3.6.1.2.1.2.2.1.17.1       → TX packets
  ifInErrors:   1.3.6.1.2.1.2.2.1.14.1         → RX errors
  ifOutErrors:  1.3.6.1.2.1.2.2.1.20.1         → TX errors
  ifInDiscards:  1.3.6.1.2.1.2.2.1.13.1        → RX discards
  ifOutDiscards: 1.3.6.1.2.1.2.2.1.19.1        → TX discards
  ifSpeed:      1.3.6.1.2.1.2.2.1.5.1          → Interface speed (bps)
  ifAdminStatus: 1.3.6.1.2.1.2.2.1.3.1         → Admin status (1=up, 2=down)
  ifOperStatus:  1.3.6.1.2.1.2.2.1.8.1         → Operational status (1=up, 2=down)
```

### 64-bit Counters (Recommended - for high-speed interfaces)

```
Base: .1.3.6.1.2.1.31.1.1.1.[metric].[ifIndex]

Example for port1 (ifIndex=1):
  ifHCInOctets:   1.3.6.1.2.1.31.1.1.1.6.1      → RX bytes (64-bit)
  ifHCOutOctets:  1.3.6.1.2.1.31.1.1.1.10.1     → TX bytes (64-bit)
  ifHCInUcastPkts: 1.3.6.1.2.1.31.1.1.1.7.1     → RX packets (64-bit)
  ifHCOutUcastPkts: 1.3.6.1.2.1.31.1.1.1.11.1   → TX packets (64-bit)
  ifHighSpeed:    1.3.6.1.2.1.31.1.1.1.15.1     → Speed in Mbps
```

## Standard MIB-II OIDs (Always Available)

| OID | Name | Description |
|-----|------|-------------|
| `1.3.6.1.2.1.1.1.0` | sysDescr | System description |
| `1.3.6.1.2.1.1.2.0` | sysObjectID | System object identifier |
| `1.3.6.1.2.1.1.3.0` | sysUpTime | System uptime |
| `1.3.6.1.2.1.1.4.0` | sysContact | Contact information |
| `1.3.6.1.2.1.1.5.0` | sysName | System name (hostname) |
| `1.3.6.1.2.1.1.6.0` | sysLocation | System location |
| `1.3.6.1.2.1.2.1.0` | ifNumber | Number of interfaces |
| `1.3.6.1.2.1.25.3.2.1.5.1` | hrDeviceDescr | Host resource device description |

## Virtual Domain Specific OIDs

For monitoring resources per virtual domain:

```
fgVdTable = 1.3.6.1.4.1.12356.101.6.3.1
  |
  +-- fgVdEntry (1)
      |
      +-- fgVdIndex (1)              → Virtual Domain ID
      +-- fgVdName (2)               → Virtual Domain Name
      +-- fgVdOpMode (3)             → Operating Mode
      +-- fgVdCpuUsage (4)           → CPU Usage %
      +-- fgVdMemUsage (5)           → Memory Usage %
      +-- fgVdSessionCount (6)       → Session Count
      +-- fgVdPolicy (7)             → Policy Count
      +-- fgVdAddress (8)            → Address Count

Example: Virtual Domain 0 CPU Usage
  OID: 1.3.6.1.4.1.12356.101.6.3.1.1.4.0
```

## Firewall Statistics OIDs

Monitor firewall policies and sessions:

```
fgFwTable = 1.3.6.1.4.1.12356.101.8

Session information:
  fgSessionCount = 1.3.6.1.4.1.12356.101.3.1.0
  fgSessionRate = 1.3.6.1.4.1.12356.101.3.2.0

Traffic statistics:
  fgPolicyInPkts = 1.3.6.1.4.1.12356.101.8.1.1.1
  fgPolicyOutPkts = 1.3.6.1.4.1.12356.101.8.1.1.2
  fgPolicyInBytes = 1.3.6.1.4.1.12356.101.8.1.1.3
  fgPolicyOutBytes = 1.3.6.1.4.1.12356.101.8.1.1.4
```

## VPN Statistics OIDs

Monitor VPN tunnel status:

```
fgVpnTable = 1.3.6.1.4.1.12356.101.12

IPSec Tunnel:
  fgVpnTunnelIndex = 1.3.6.1.4.1.12356.101.12.1.1.1
  fgVpnTunnelName = 1.3.6.1.4.1.12356.101.12.1.1.2
  fgVpnTunnelStatus = 1.3.6.1.4.1.12356.101.12.1.1.3 (0=down, 1=up)
  fgVpnTunnelInPkts = 1.3.6.1.4.1.12356.101.12.1.1.4
  fgVpnTunnelOutPkts = 1.3.6.1.4.1.12356.101.12.1.1.5
```

## Antenna Signal Strength (Wi-Fi models)

```
fgWifiTable = 1.3.6.1.4.1.12356.101.14

fgWifiEntry:
  fgWifiIndex = 1.3.6.1.4.1.12356.101.14.1.1.1
  fgWifiSsid = 1.3.6.1.4.1.12356.101.14.1.1.2
  fgWifiSignal = 1.3.6.1.4.1.12356.101.14.1.1.3
  fgWifiClients = 1.3.6.1.4.1.12356.101.14.1.1.4
```

## Temperature Sensors (Hardware monitors)

```
fgSensorTable = 1.3.6.1.4.1.12356.101.15

fgSensorEntry:
  fgSensorIndex = 1.3.6.1.4.1.12356.101.15.1.1.1
  fgSensorName = 1.3.6.1.4.1.12356.101.15.1.1.2
  fgSensorValue = 1.3.6.1.4.1.12356.101.15.1.1.3
  fgSensorThreshold = 1.3.6.1.4.1.12356.101.15.1.1.4
```

## Power Supply Status

```
fgPowerSupplyStatus = 1.3.6.1.4.1.12356.101.16
  |
  +-- fgPowerSupplyStatus1 = 1.3.6.1.4.1.12356.101.16.1.0
  +-- fgPowerSupplyStatus2 = 1.3.6.1.4.1.12356.101.16.2.0
```

## Fan Status

```
fgFanStatus = 1.3.6.1.4.1.12356.101.17
  |
  +-- fgFanStatus1 = 1.3.6.1.4.1.12356.101.17.1.0 (0=OK, 1=Failed)
  +-- fgFanStatus2 = 1.3.6.1.4.1.12356.101.17.2.0
```

## SMON Implementation Status

| OID | Feature | Status | Notes |
|-----|---------|--------|-------|
| fgSysCpuUsage | CPU Monitoring | ✅ Implemented | Automated polling |
| fgSysMemUsage | Memory Monitoring | ⏳ Available OID | Requires frontend implementation |
| fgSysUptime | System Uptime | ✅ Implemented | Via standard sysUpTime |
| fgSysSerialNumber | Device Serial | ✅ Available | Displayed in device info |
| fgHaStatus | HA Monitoring | ✅ Available OID | Can be queried |
| Interface Stats | Bandwidth | ✅ Implemented | Full 32-bit and 64-bit support |
| fgSessionCount | Session Count | ⏳ Available | Can be added if needed |
| fgVdCpuUsage | Per-VD CPU | ⏳ Available | Can be added for multi-VD monitoring |

## Testing OID Availability

### Using snmpwalk

```bash
# Walk all Fortigate objects
snmpwalk -v2c -c public 192.168.1.100 1.3.6.1.4.1.12356

# Walk system info only
snmpwalk -v2c -c public 192.168.1.100 1.3.6.1.4.1.12356.101.4.1

# Walk interface stats
snmpwalk -v2c -c public 192.168.1.100 1.3.6.1.2.1.2.2.1

# Walk 64-bit interface counters
snmpwalk -v2c -c public 192.168.1.100 1.3.6.1.2.1.31.1.1.1
```

### Using snmpget

```bash
# Get CPU usage
snmpget -v2c -c public 192.168.1.100 1.3.6.1.4.1.12356.101.4.1.3.0

# Get memory usage
snmpget -v2c -c public 192.168.1.100 1.3.6.1.4.1.12356.101.4.1.4.0

# Get system uptime
snmpget -v2c -c public 192.168.1.100 1.3.6.1.4.1.12356.101.4.1.1.0

# Get serial number
snmpget -v2c -c public 192.168.1.100 1.3.6.1.4.1.12356.101.4.1.2.0

# Get interface count
snmpget -v2c -c public 192.168.1.100 1.3.6.1.2.1.2.1.0
```

## MIB Files Download

Official Fortigate MIB files can be downloaded from:
- Fortinet Support Site: https://support.fortinet.com/
- Device Web UI: System → Settings → MIB Download
- Search: "FORTINET-FORTIGATE-MIB"

Common MIB files:
- `FORTINET-FORTIGATE-MIB.mib` - Main Fortigate MIB
- `FORTINET-CORE-MIB.mib` - Core Fortinet definitions
- `FORTINET-CORE-MIB.my` - Alternative format
