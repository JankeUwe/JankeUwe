# SQL Automation Suite

A set of tools that cover the SQL Server lifecycle end to end: install to a standard,
run it day to day, make it highly available, migrate it, and document all of it. Every
tool stands on its own, and they interlock where that actually helps.

The common ground is [dbatools](https://dbatools.io). Where dbatools gives you the
individual commands, these tools add the operational process around them — the GUI, the
runbook, and the HTML report that proves what happened.

---

## Lifecycle

How the tools follow one another in practice:

```
        SQLSetupTool                Standardized installation
             |
             v
         sqmSQLTool                 Day-to-day administration
             |
   +---------+---------+
   |         |         |
   v         v         v
AlwaysOn  Migration  Partition      Specialized tasks
   |         |         |
   +---------+---------+
             |
             v
   HTML reports / GUI               Evidence and documentation
```

## Technical dependencies

How the tools actually depend on each other — deliberately kept separate from the
lifecycle, because the two are not the same graph:

```
dbatools
   |
   +-- sqmSQLTool                   RequiredModules: dbatools
   |        |
   |        +-- sqmPartitionTool    RequiredModules: dbatools + sqmSQLTool >= 1.9.2.0
   |        +-- SQLSetupTool        Imports sqmSQLTool at runtime
   |
   +-- AlwaysOnSetup                uses dbatools directly

SQLMigration                        standalone
```

Every other tool runs independently and requires neither sqmSQLTool nor any of the others.

---

## Components

| Project | Purpose |
| ------- | ------- |
| [SQLSetupTool](https://github.com/JankeUwe/SQLSetupTool) | Fully automated, standardized SQL Server installation and configuration (WinForms) |
| [sqmSQLTool](https://github.com/JankeUwe/sqmSQLTool) | Administration framework: 155 functions for reporting, health checks, maintenance and security auditing ([PowerShell Gallery](https://www.powershellgallery.com/packages/sqmSQLTool)) |
| [AlwaysOnSetup](https://github.com/JankeUwe/AlwaysOnSetup) | Fully automated setup of AlwaysOn availability groups |
| [SQLMigration](https://github.com/JankeUwe/SQLMigration) | Two-phase SQL Server migration across separated networks |
| [sqmPartitionTool](https://github.com/JankeUwe/sqmPartitionTool) | Table partitioning with an automatic sliding window (GUI and CLI) |
| [InplaceUpDate](https://github.com/JankeUwe/InplaceUpDate) | In-place upgrades that back up every dependency, with a runbook |
| [SSRSDeploymentTool](https://github.com/JankeUwe/SSRSDeploymentTool) | SSRS report deployment through the REST API v2.0 |
| [ReportServerCheck](https://github.com/JankeUwe/ReportServerCheck) | 20 RDL diagnostic reports for SSRS and Power BI Report Server |
| [DeadlockCollector](https://github.com/JankeUwe/DeadlockCollector) | Automatic deadlock capture from the system_health XEvent session |
| [OperationsManager](https://github.com/JankeUwe/OperationsManager) | Reporting layer over the SCOM OperationsManagerDW with SQL Server inventory |
| [SsisAnalyzer](https://github.com/JankeUwe/SsisAnalyzer) | SSIS analysis and documentation: control flow, version comparison, data lineage (.NET 9) |
| [SqlRefactorAnalyzer](https://github.com/JankeUwe/SqlRefactorAnalyzer) | T-SQL refactoring and execution plan analysis, strictly read-only (C#/WinForms) — [binaries](https://github.com/JankeUwe/SqlRefactorAnalyzerEXE) |
| [TDPBackup](https://github.com/JankeUwe/TDPBackup) | Backup management for SQL Server with TDP/TSM, one job for every scenario |

---

## What defines them

- **PowerShell 5.1+** — runs on Windows PowerShell 5.1 and PowerShell 7
- **GUI and CLI** — a WinForms interface for the rare case, the command line for the regular one
- **Offline capable** — installation and module sideload without internet access, from a UNC path or a local directory
- **Multi-domain** — built for separated domains with no trust between them
- **HTML reporting** — every tool produces self-contained HTML reports as evidence
- **Built on dbatools** — extends the dbatools commands into complete operational processes

## In production

These are not lab tools. They run in a strictly regulated, network-segmented
large-enterprise environment:

- A four-digit number of SQL Server instances
- A five-digit number of databases
- Many separate security domains with no mutual trust
- Deployment entirely offline
- HTML management reports as operational evidence

---

## More

Detailed write-ups, screenshots and presentations for the individual tools:
**[powershelldba.de](https://www.powershelldba.de)**
