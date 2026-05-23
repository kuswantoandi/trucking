```mermaid
stateDiagram-v2
    [*] --> ARRIVED : Scan Kedatangan Verifikasi Supir dan Truck
    ARRIVED --> LOADED : Pengisian Pasir dan Input Tujuan Kunci Snapshot Harga
    LOADED --> DEPARTED : Scan Keberangkatan dan Konfirmasi Penyerahan Uang Jalan
    DEPARTED --> DELIVERED : Scan Konfirmasi Sampai di Tujuan Akhir
    DELIVERED --> [*] : Profit Terfinalisasi dan Masuk Laporan Keuangan
