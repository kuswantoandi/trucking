```mermaid
graph TD
    classDef online fill:#2ecc71,stroke:#27ae60,stroke-width:2px,color:#fff;
    classDef offline fill:#e67e22,stroke:#d35400,stroke-width:2px,color:#fff;
    
    Scan[Operator Melakukan Scan Barcode] --> CheckNet{Apakah Ada<br>Koneksi Internet?}
    
    CheckNet -- Ada / Online --> PostAPI[Kirim Data Langsung ke API NestJS]:::online
    PostAPI --> UpdateDB[Database PostgreSQL Terupdate]
    
    CheckNet -- Tidak / Offline --> SaveLocal[Simpan Data ke Local Cache DB<br>Isar / Hive / Drift]:::offline
    SaveLocal --> Queue[Antrean Sinkronisasi Aktif]:::offline
    
    Queue --> LoopCheck{Cek Koneksi Berkala...<br>Kembali Online?}
    LoopCheck -- Belum --> Queue
    LoopCheck -- Ya --> SyncData[Otomatis Sinkronisasi Data Antrean ke Server]:::online
    SyncData --> UpdateDB
