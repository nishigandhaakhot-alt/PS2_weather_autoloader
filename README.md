# PS2_weather_autoloader

How can we automatically ingest daily CSV files from a storage folder into a Delta table using Databricks Auto Loader?

In many real data platforms, systems drop new files into storage every day. Instead of manually processing them, we can build pipelines that automatically detect and ingest new files.

I built a small pipeline using Databricks Auto Loader to handle this.

What the pipeline does:

• Detects new CSV files arriving in a storage folder
• Uses Auto Loader to incrementally ingest only new files
• Tracks processed files using checkpointing
• Writes data into a Delta table for downstream processing

Under the hood:

• `spark.readStream` continuously monitors the folder for new files
• `cloudFiles` (Auto Loader) efficiently discovers new files at scale
• Checkpointing ensures files are not reprocessed
• Data is stored in Delta format for reliability and ACID transactions

A couple of interesting things I learned while building this:

• Auto Loader tracks processed files using checkpoints, so if a file with the same name appears again it will not be reprocessed.
• Multiple CSV files are automatically appended into a single Delta table, creating a scalable ingestion layer.

Architecture:

Storage Folder → Databricks Auto Loader → Delta Table

Tech stack:
Azure Databricks | PySpark | Delta Lake

