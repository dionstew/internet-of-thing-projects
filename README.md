# Internet of Things Projects
Ini adalah repository untuk semua tim mengirimkan progress sistem dari keseluruhan proyek IoT.
Semua kelompok wajib mengupload file-file baik program, dashboard dan setup MQTT ke laman ini.
Laman ini bisa digunakan juga sebagai repo cadangan anda untuk menyimpan program dan update progress pengerjaan proyek anda.

## Cara menggunakan
1.  Ajukan branch baru pada repository ini dengan nama kelompok Anda atau nama proyek anda.
2.  Clone repository dengan menggunakan tag "-b" \n
   `git clone https://github.com/dionstew/internet-of-thing-projects.git -b <nama-branch>`

## Aturan Pada Masing-masing branch atau kelompok
Harus mengatur:
1. file-tree
2. README-personal.md
3. Topic-map
4. Data contract
5. Checklist
   
### Konsep file-tree dari project anda harus sebagai berikut

```iot-of-things-projects/
├─ README.md                                 (README.md default dari repsitory saya- Jangan dihapus!!!)
├─ README-personal.md                        (README.md per kelompok, silahkan diisi sesuai template dibawah !!!)
├─ LICENSE
├─ .gitignore
├─ docs/
│  ├─ 00-quickstart.md
│  ├─ 01-architecture.md
│  ├─ 02-topic-map.md
│  ├─ 03-data-contract.md
│  ├─ 04-security.md
│  ├─ 05-testing.md                  (Opsional)
│  ├─ submit-checklist.md
│  └─ screenshots/
├─ firmware/
│  ├─ arduino/
│  │  └─ nama-program-anda                    (Workspace Arduino - Nama file disesuaikan dengan keinginan)
│  │  │  └─ nama-program-anda.ino             (Program Arduino   - Nama file disesuaikan dengan keinginan)
├─ backend/
│  ├─ nodered/
│  │  ├─ flows.json                  (export dari Node-RED)
│  │  └─ README.md
├─ infra/
│  ├─ docker-compose.yml
│  └─ mosquitto/
│     ├─ mosquitto.conf
│     └─ pwfile                      (Opsional)
└─ tests/                            (Opsional-Kondisional)
   ├─ failure-testcases.md
   └─ evidence/```
```

### Pada README-personal.md
```
# IoT Project - <Nama Kelompok / Judul>

## 1. Ringkas
**Use case:** <monitoring/kontrol apa>  
**Target user:** <siapa>  
**Value:** <kenapa penting>  

## 2. Anggota & Peran
| Nama | NIM | Peran |
|------|-----|------|
| ...  | ... | Firmware |
| ...  | ... | Backend / Node-RED |
| ...  | ... | Dashboard / DB |

## 3. Arsitektur (End-to-End)
Lihat: `docs/01-architecture.md`  
- Device: <ESP32/ESP8266>  
- Protocol: MQTT  
- Broker: <local/cloud>  
- Storage: <InfluxDB/DB lain>  
- Dashboard: <Grafana/Node-RED dashboard>  
```

### Pada topic-map
```

---

## 3) docs/02-topic-map.md

```md
# Topic Map (MQTT)

## Naming rules
- Format umum: `iot/<course>/<deviceId>/<channel>`
- Semua payload JSON
- Semua message MUST contain `deviceId` dan `ts`

## Topics
### 1) Telemetry (device -> cloud)
- Topic: `iot/iot101/<deviceId>/telemetry`
- QoS: 0/1 (jelaskan pilihan)
- Retained: false

### 2) Status (device -> cloud)
- Topic: `iot/iot101/<deviceId>/status`
- QoS: 1
- Retained: true (biar last known status kebaca)
- LWT Topic: `iot/iot101/<deviceId>/status`
- LWT Payload: `{"deviceId":"...","ts":...,"state":"offline"}`

### 3) Command (cloud -> device)
- Topic: `iot/iot101/<deviceId>/command`
- QoS: 1
- Retained: false

### 4) Command Ack (device -> cloud)
- Topic: `iot/iot101/<deviceId>/command_ack`
- QoS: 1
- Payload includes `cmdId` dan `result`

## Examples
### Telemetry
Topic: `iot/iot101/esp32-A1/telemetry`
Payload:
```json
{
  "v": 1,
  "deviceId": "esp32-A1",
  "ts": 1710000000,
  "sensors": {
    "tempC": 29.1,
    "hum": 70.2,
    "adc": 812
  },
  "status": { "uptimeS": 120, "rssi": -55 }
}
```

### Pada data-contract
```

## 4) docs/03-data-contract.md 

```md
# Data Contract v1

## Common fields (wajib)
- `v` (int): versi payload
- `deviceId` (string)
- `ts` (int): epoch seconds
- `status` (object): minimal uptime + rssi (kalau ada)

## Telemetry fields
- `sensors.tempC` (float, Celsius)
- `sensors.hum` (float, %)
- `sensors.*` (opsional sesuai sensor)

## Status message
- `state`: "online" | "offline"
- `fw`: versi firmware (opsional)
- `ip`: ip address (opsional)

## Command message
- `cmdId` (string) unik
- `type`: "setInterval" | "setThreshold" | ...
- `params`: object

## Validation rules (wajib)
- `ts` tidak boleh null
- nilai sensor harus range-checked (tulis range tiap sensor)
- jika sensor error: isi `sensors.<x> = null` + tambahkan `status.errorCodes`
```

### Pada checklist.md
```
# Submit Checklist (Wajib)

- [ ] Repo rapi, bisa dibuild ulang
- [ ] Topic map lengkap + konsisten dengan implementasi
- [ ] Data contract v1 ada + contoh payload
- [ ] Node-RED flow export disertakan
- [ ] Dashboard
- [ ] Ada command + ack (remote config)
- [ ] Ada LWT + retained status
- [ ] Video demo unlisted ada link
```

## Kesimpulan
Repository ini wajib digunakan oleh semua kelompok untuk menyimpan, update dan memfinalisasi program dari project IoT anda sebagai bahan penilaian. Selain itu program yang telah anda kumpulkan di dalam repo ini dapat digunakan kembai jika anda memerlukannya di lain waktu.
