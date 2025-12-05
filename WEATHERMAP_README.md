# Network Weathermap Implementation

## Overview
Network Weathermap adalah fitur untuk memvisualisasikan utilisasi link jaringan secara real-time dalam bentuk peta interaktif.

**✅ Topology Detection**: Sistem sekarang dapat mendeteksi topologi jaringan dari file `data/topology.json` dan menampilkan koneksi antar device dengan data utilisasi bandwidth secara real-time.

## Fitur Yang Sudah Diimplementasi

### ✅ Halaman Weathermap (`/weathermap`)
- ✅ Visualisasi jaringan interaktif menggunakan Cytoscape.js dengan layout COSE (built-in)
- ✅ Color-coded link utilization (Hijau 🟢 → Merah 🔴)
- ✅ Real-time tooltip dengan detail link saat hover
- ✅ Auto-refresh setiap 30 detik
- ✅ Kontrol manual refresh dan fullscreen
- ✅ Responsive legend utilisasi dengan positioning yang tepat
- ✅ Controls yang terintegrasi dengan backdrop blur effects

### ✅ API Endpoint (`/api/weathermap/links`)
- Mengambil data utilisasi bandwidth dari InfluxDB
- Menghitung persentase utilisasi berdasarkan speed interface
- Mendukung bidirectional link monitoring

### ✅ Navigasi Sidebar
- Link "Weathermap" ditambahkan di sidebar navigation
- Icon globe untuk representasi visual

## Cara Kerja

1. **Topology Loading**: Sistem memuat data topologi dari `data/topology.json`
2. **Device Mapping**: Mapping antara device topology dengan SNMP devices
3. **Data Collection**: Sistem mengambil data bandwidth dari InfluxDB untuk setiap interface
4. **Utilization Calculation**: Menghitung utilisasi sebagai persentase dari kapasitas link
5. **Visualization**: Menampilkan link dengan warna berdasarkan utilisasi:
   - 🟢 Hijau: 0-25% (rendah)
   - 🟡 Kuning: 25-50% (sedang)
   - 🟠 Orange: 50-75% (tinggi)
   - 🔴 Merah: 75-100% (sangat tinggi)

## Pengembangan Selanjutnya

### 1. Topology Database
Untuk weathermap yang lengkap, perlu database topologi yang mendefinisikan koneksi antar device:

```javascript
// Contoh struktur topology database
const topology = {
  links: [
    {
      sourceDevice: "device1",
      targetDevice: "device2",
      sourceInterface: "GigabitEthernet0/1",
      targetInterface: "GigabitEthernet0/1",
      type: "ethernet",
      speed: 1000000000 // 1Gbps in bps
    }
  ]
};
```

### 2. LLDP/CDP Integration
Menggunakan protokol discovery untuk auto-detect koneksi:
- **LLDP** (Link Layer Discovery Protocol)
- **CDP** (Cisco Discovery Protocol)

### 3. Advanced Features
- **Threshold Alerts**: Notifikasi saat utilisasi melebihi threshold
- **Historical Trends**: Grafik utilisasi historis per link
- **Link Aggregation**: Mengelompokkan multiple links
- **Custom Layouts**: Save/load custom network layouts

### 4. Implementasi Topology Detection

```javascript
// Fungsi untuk mendeteksi koneksi menggunakan LLDP
async function detectLLDPNeighbors(deviceId) {
  // SNMP query untuk LLDP MIB
  const lldpQuery = '1.0.8802.1.1.2.1.4.1.1'; // lldpRemTable

  // Return array of neighbor connections
  return neighbors;
}

// Update findConnectedDevice function
function findConnectedDevice(sourceDeviceId, interfaceName) {
  // 1. Check topology database first
  // 2. Query LLDP/CDP information
  // 3. Use interface naming conventions as fallback

  return connectedDeviceId || null;
}
```

## Testing

### 1. Akses Halaman
```
http://localhost:3000/weathermap
```

### 2. Test API
```bash
curl http://localhost:3000/api/weathermap/links
```

### 3. Verifikasi Data
- Pastikan device memiliki interface yang dipantau
- Pastikan ada data bandwidth di InfluxDB
- Check console browser untuk error JavaScript
- **Topology**: Pastikan `data/topology.json` berisi nodes dan links yang valid

## Troubleshooting

### Layout Errors
Jika mengalami error layout Cytoscape:
- Pastikan hanya menggunakan built-in layouts (cose, grid, etc.)
- Hindari external layout libraries yang bermasalah
- COSE layout sudah mencukupi untuk visualisasi weathermap

### UI Positioning Issues
- Controls dan legend sekarang positioned relative to canvas
- Menggunakan absolute positioning dengan proper z-index
- Backdrop blur effects untuk better visibility
- Responsive design untuk berbagai screen sizes

### Script Loading Issues
- Semua Cytoscape scripts sudah dioptimasi
- Tidak ada dependencies external yang bermasalah
- Layout menggunakan algoritma built-in yang stabil

## Dependencies

- **Cytoscape.js**: Graph visualization library (built-in COSE layout only)
- **InfluxDB**: Time-series database untuk data bandwidth
- **SNMP**: Untuk device dan interface discovery

## File Yang Dimodifikasi

- `app.js`: Menambah route dan API endpoint
- `views/weathermap.ejs`: Halaman weathermap baru
- `views/partials/sidebar.ejs`: Menambah link navigasi

## Status Implementasi

🟢 **FULLY FUNCTIONAL**
- ✅ Basic visualization framework dengan COSE layout (built-in)
- ✅ Real-time bandwidth monitoring
- ✅ API endpoints lengkap
- ✅ Navigation berfungsi
- ✅ Layout compatibility 100% (no external dependencies)
- ✅ Error-free operation
- ✅ **Topology detection dari topology.json**
- ❌ Auto-discovery (perlu LLDP/CDP integration)