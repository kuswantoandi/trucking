```mermaid
graph TD
    %% Setup Style
    classDef startEnd fill:#2ecc71,stroke:#27ae60,stroke-width:2px,color:#fff;
    classDef process fill:#3498db,stroke:#2980b9,stroke-width:2px,color:#fff;
    classDef decision fill:#f1c40f,stroke:#f39c12,stroke-width:2px,color:#000;
    classDef alert fill:#e74c3c,stroke:#c0392b,stroke-width:2px,color:#fff;

    Start([Truck Tiba di<br>Pos Loading]) --> Scan1[1. Operator Scan<br>Barcode Truck]:::process
    
    Scan1 --> CheckActive{Apakah Barcode<br>& Supir Aktif?}:::decision
    CheckActive -- Tidak --> AlertActive[Tolak Scan &<br>Perbaiki Master]:::alert
    CheckActive -- Ya --> VerifyDriver{Verifikasi Foto<br>Supir Sesuai?}:::decision
    
    VerifyDriver -- Tidak Sesuai --> AlertDriver[Ganti Supir /<br>Butuh Approval]:::alert
    AlertDriver --> VerifyDriver
    
    VerifyDriver -- Sesuai --> StateArrived[Status Ritase:<br>ARRIVED]:::process
    
    StateArrived --> LoadPasir[2. Muat Pasir<br>Default: 6 m3]:::process
    LoadPasir --> InputTujuan[Operator Input<br>Tujuan]:::process
    
    InputTujuan --> SystemSnapshot[Snapshot Harga &<br>Hitung Subtotal]:::process
    SystemSnapshot --> StateLoaded[Status Ritase:<br>LOADED]:::process
    
    StateLoaded --> Scan2[3. Scan Kedua<br>Pemberangkatan]:::process
    Scan2 --> ShowUangJalan[Sistem Tampilkan<br>Nominal Uang Jalan]:::process
    ShowUangJalan --> PayUangJalan[Finance Serahkan<br>Uang Jalan]:::process
    
    PayUangJalan --> ConfirmPay{Konfirmasi<br>Uang Jalan?}:::decision
    ConfirmPay -- Tidak --> BlockDepart[Sistem Blokir<br>Status]:::alert
    ConfirmPay -- Ya --> StateDeparted[Status Ritase:<br>DEPARTED]:::process
    
    StateDeparted --> Travel[Perjalanan ke<br>Destinasi]:::process
    
    Travel --> CheckTime{Waktu Tempuh<br>> 24 Jam?}:::decision
    CheckTime -- Ya --> ExceptionList[Masuk Daftar<br>Exception / Alert]:::alert
    CheckTime -- Tidak --> ArriveDest[4. Tiba di Tujuan]:::process
    ExceptionList --> ArriveDest
    
    ArriveDest --> Scan3[Operator Checker<br>Scan Barcode]:::process
    Scan3 --> StateDelivered[Status Ritase:<br>DELIVERED]:::process
    
    StateDelivered --> FinalizeFinance[Finalisasi Keuangan:<br>Profit = Rev - Cost]:::process
    
    FinalizeFinance --> End([Selesai / Data<br>Masuk Dashboard]):::startEnd

    class Start,End startEnd;
