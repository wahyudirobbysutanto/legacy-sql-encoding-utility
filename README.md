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
# Read legacy encoded file
$content = Get-Content -LiteralPath "C:\your_path\legacy_file.sql" -Raw

# Overwrite with standard UTF-8 (No BOM)
[System.IO.File]::WriteAllText("C:\your_path\legacy_file.sql", $content, [System.Text.Encoding]::UTF8)
