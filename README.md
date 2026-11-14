# TCBA: Traffic Congestion Big Data Analytics & Predictive Lakehouse

Sistem analitik *end-to-end* berbasis arsitektur Medallion Lakehouse di Microsoft Azure untuk menganalisis pola kemacetan lalu lintas global menggunakan dataset historis TomTom (ForExportNewApi.csv, 1,23 GB / 7+ juta baris, periode 2025–2026).

---

## Arsitektur Sistem

    [Kaggle Dataset (1.23 GB CSV)]
            │
            ▼ (Ingesti & Partisi year/month)
    [ADLS Gen2: Container `lakehouse`]
      ├── bronze/tomtom_partitioned/   
      ├── silver/                      
      └── gold/                        

---

## Pembagian Peran Tim

| Anggota | Peran |
| :--- | :--- |
| Jimly Syahbatin | Ingestion & partisi data ke ADLS Gen2, konfigurasi RBAC/Managed Identity, FinOps (Cost Alert & Auto-kill Spark Pool via Livy API), serta pengelolaan GitHub & CI/CD. |
| Najma | Pembersihan data & transformasi PySpark, optimasi Delta Lake |
| Adel | Feature engineering (lag features, profil waktu), pelatihan & evaluasi model, tracking MLflow, dan deployment REST API (FastAPI). |
| Darrel | Integrasi Power BI dengan Synapse Serverless SQL

---

## Struktur Repositori

    TCBA_Traffic-Congestion-Analysis/
    ├── .github/workflows/ci-cd.yml   # GitHub Actions (Flake8, Validasi JSON & Sync ADLS)
    ├── adf/                          # Pipeline & Linked Services
    ├── notebook/                     # Notebook PySpark (Partisi Bronze, ETL Silver-Gold, Training ML)
    ├── pipeline/                     # Pipeline Azure Synapse Analytics
    ├── sqlscript/                    # Skrip SQL untuk External Table & View di Synapse Serverless
    ├── api/                          # Aplikasi FastAPI untuk REST API Inference model ML
    ├── .gitignore                    # Pengecualian file dataset besar (.csv/.parquet) & kredensial
    └── README.md                     # Dokumentasi arsitektur & panduan reproduksi proyek