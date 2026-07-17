# Legacy SQL File Encoding Utility (PowerShell Manual Snippet)

A conceptual guide and manual script to convert legacy SQL files (ANSI, Windows-1252) into Standard UTF-8 to prevent deployment failures in modern Linux-based Docker containers and Git-based CI/CD pipelines.

## The Problem
Many legacy extraction tools (such as older SSMS versions or custom DB dumpers) export Stored Procedures, DDL schemas, and tables in ANSI, Windows-1252, or UTF-16 formats. 

When these raw files are pushed into cloud database environments (Snowflake, BigQuery, AWS Redshift) or executed inside Linux containers, the encoding mismatch causes:
* Broken string literals and corrupted special characters.
* Immediate pipeline failures in automated Git-based deployment workflows.
* Inconsistent database migrations.

---

## Manual Resolution (PowerShell Snippet)

To convert a single file manually, execute the following commands in your local Windows PowerShell terminal:

```powershell
# Required for PowerShell 7+ to support legacy Windows-1252 encoding
[System.Text.Encoding]::RegisterProvider([System.Text.CodePagesEncodingProvider]::Instance)

# Read legacy encoded file using explicit Windows-1252 (ANSI) decoding
$content = [System.IO.File]::ReadAllText("C:\your_path\legacy_file.sql", [System.Text.Encoding]::GetEncoding(1252))

# Overwrite with standard UTF-8 (No BOM)
[System.IO.File]::WriteAllText("C:\your_path\legacy_file.sql", $content, [System.Text.Encoding]::UTF8)
```

### ⚠️ Manual Method Risks:
* **No Safety Net:** This script instantly overwrites the source file. If your system runs out of disk space or crashes mid-write, your SQL code is lost.
* **No Automation:** You must manually write loops, manage nested directory path objects, and handle system execution errors.
* **No Log Auditing:** There is no standard logging mechanism to verify which schemas succeeded or failed during bulk migration.

---

## 🚀 Enterprise Automation Utility (Production-Ready)

For large-scale production migrations requiring automated execution across multi-level subdirectories with strict safety protocols, use the optimized, production-ready execution package.

### Advanced Features of the Enterprise Package:
* **Interactive Smart Prompt:** Supports drag-and-drop folder inputs with automatic active-directory fallback options if paths are left empty.
* **Isolated Out-of-Place Output:** Configure separate destination folders to avoid touching production systems directly.
* **Fail-Safe Rollback Layer:** Automatically clones mirror directories (`_backup_original_`) containing legacy schemas before modifying any code.
* **Audit Logs:** Generates standardized `migration_log.txt` detailing every converted procedure and tracing exception reasons.

👉 **[Download the Zero-Dependency Enterprise SQL Conversion Package on Gumroad ($9.00)](https://wahyudirobbysutanto.gumroad.com/l/sql-utf8-migration-utility)**
