'''mermaid
graph TD
    %% Setup Style
    classDef startEnd fill:#2ecc71,stroke:#27ae60,stroke-width:2px,color:#fff;
    classDef process fill:#3498db,stroke:#2980b9,stroke-width:2px,color:#fff;
    classDef decision fill:#f1c40f,stroke:#f39c12,stroke-width:2px,color:#000;
    classDef alert fill:#e74c3c,stroke:#c0392b,stroke-width:2px,color:#fff;

    Start([Truck Tiba di Pos Loading]) --> Scan1[1. Operator Scan Barcode Truck]:::process
    
    Scan1 --> CheckActive{Apakah Barcode &<br>Supir Aktif?}:::decision
    CheckActive -- Tidak/Ditolak --> AlertActive[Tolak Scan & Perbaiki Master Data]:::alert
    CheckActive -- Ya --> VerifyDriver{Verifikasi Foto Supir<br>Sesuai dengan Fisik?}:::decision
    
    VerifyDriver -- Tidak Sesuai --> AlertDriver[Ganti Supir Cepat /<br>Butuh Approval Manager]:::alert
    AlertDriver --> VerifyDriver
    
    VerifyDriver -- Sesuai --> StateArrived[Status Ritase: ARRIVED]:::process
    
    StateArrived --> LoadPasir[2. Muat Pasir ke Truck<br>Default: 6 m3]:::process
    LoadPasir --> InputTujuan[Operator Input/Konfirmasi Tujuan]:::process
    
    InputTujuan --> SystemSnapshot[Sistem Snapshot Harga & Uang Jalan<br>Hitung Subtotal Pendapatan]:::process
    SystemSnapshot --> StateLoaded[Status Ritase: LOADED]:::process
    
    StateLoaded --> Scan2[3. Scan Barcode Kedua<br>Pemberangkatan]:::process
    Scan2 --> ShowUangJalan[Sistem Menampilkan Nominal Uang Jalan]:::process
    ShowUangJalan --> PayUangJalan[Finance/Operator Serahkan Uang Jalan]:::process
    
    PayUangJalan --> ConfirmPay{Konfirmasi Pengeluaran<br>Uang Jalan?}:::decision
    ConfirmPay -- Tidak --> BlockDepart[Sistem Blokir /<br>Tidak Bisa Departed]:::alert
    ConfirmPay -- Ya --> StateDeparted[Status Ritase: DEPARTED<br>Catat Timestamp & Biaya]:::process
    
    StateDeparted --> Travel[Perjalanan Menuju Destinasi]:::process
    
    Travel --> CheckTime{Waktu Tempuh<br>> 24 Jam?}:::decision
    CheckTime -- Ya --> ExceptionList[Masuk Daftar Exception /<br>Alert di Dashboard Web]:::alert
    CheckTime -- Tidak --> ArriveDest[4. Tiba di Tujuan]:::process
    ExceptionList --> ArriveDest
    
    ArriveDest --> Scan3[Operator/Checker Tujuan Scan Barcode]:::process
    Scan3 --> StateDelivered[Status Ritase: DELIVERED]:::process
    
    StateDelivered --> FinalizeFinance[Finalisasi Cash Flow Ritase:<br>1. Revenue = Kubikasi x Harga<br>2. Cost = Uang Jalan<br>3. Profit = Revenue - Cost]:::process
    
    FinalizeFinance --> End([Selesai / Data Masuk Dashboard]):::startEnd

    class Start,End startEnd;
