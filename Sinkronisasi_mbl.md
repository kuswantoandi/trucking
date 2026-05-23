```mermaid
graph TD
    %% Setup Style
    classDef online fill:#2ecc71,stroke:#27ae60,stroke-width:2px,color:#fff;
    classDef offline fill:#e67e22,stroke:#d35400,stroke-width:2px,color:#fff;
    
    Scan[Operator Lapangan<br>Scan Barcode] --> CheckNet{Apakah Ada<br>Koneksi?}
    
    CheckNet -- Ya / Online --> PostAPI[Kirim Langsung<br>ke API NestJS]:::online
    PostAPI --> UpdateDB[Database Postgres<br>Terupdate]
    
    CheckNet -- Tidak / Offline --> SaveLocal[Simpan ke Cache<br>Isar / Hive DB]:::offline
    SaveLocal --> Queue[Antrean Sinkronisasi<br>Mulai Aktif]:::offline
    
    Queue --> LoopCheck{Cek Berkala:<br>Sinyal Kembali?}
    LoopCheck -- Belum --> Queue
    LoopCheck -- Ya --> SyncData[Otomatis Sinkronisasi<br>Data ke Server]:::online
    SyncData --> UpdateDB
