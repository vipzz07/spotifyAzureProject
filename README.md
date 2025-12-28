# Spotify Azure Project

Multi-Table Incremental Ingestion Pipeline with Backfill Support and Post-Execution Alerts

This pipeline facilitates metadata-driven incremental data ingestion from multiple Azure SQL tables into Azure Data Lake, with automatic CDC (Change Data Capture) watermark tracking for each table, optional historical backfilling, and post-run alert notifications.

Lookup – TableDetails
Retrieves the metadata configuration containing the list of source tables and associated ingestion parameters.

ForEach – Table Iteration
Iterates through each table defined in the metadata and processes them individually:

Lookup – last_cdc
Obtains the previously stored CDC watermark value for the current table.
Enables resume-from-last-state behavior and prevents re-ingestion of processed records.

Copy Activity – azureSQLToLake
Performs the data ingestion from Azure SQL to the Data Lake using the condition cdc_column > last_cdc, ensuring only newly updated or inserted records are copied.

If Condition – IncrementalData
Evaluates whether new rows were ingested:

True: updates the CDC watermark to the most recent value found in the current batch

False: retains the existing watermark, indicating no new data was detected.

Backfill Support
The pipeline allows historical data backfilling by resetting the CDC watermark to an earlier timestamp or null value through metadata configuration. This enables controlled reprocessing of past data without modifying the pipeline structure.

Web Activity – Alerts
Triggered after all tables have been processed, sending an operational summary notification (via Logic App or webhook) to provide visibility into ingestion status and outcomes.

Key Capabilities: scalable multi-table orchestration, incremental ingestion with reliable watermarking, configurable historical backfilling, and integrated alerting for operational monitoring.

<img width="617" height="239" alt="image" src="https://github.com/user-attachments/assets/fe08b291-6a48-42dd-bbcd-ff5354f5c183" />

<img width="605" height="299" alt="image" src="https://github.com/user-attachments/assets/a2234c10-eae5-43b6-ad37-51bf5c4206a7" />



