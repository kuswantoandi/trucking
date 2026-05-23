```mermaid
stateDiagram-v2
    [*] --> ARRIVED : Scan Kedatangan<br>Verifikasi Supir & Truck
    ARRIVED --> LOADED : Muat Pasir & Input Tujuan<br>Kunci Snapshot Harga
    LOADED --> DEPARTED : Scan Berangkat<br>Konfirmasi Uang Jalan
    DEPARTED --> DELIVERED : Scan Checker Tujuan<br>Konfirmasi Sampai
    DELIVERED --> [*] : Profit Terfinalisasi<br>& Masuk Cash Flow
