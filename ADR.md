# 0001. Add DB to Dev Environment  

**Date:** 27-11-2025

**Status:** Accepted

## Context

A database is required for the development environment because the application code heavily relies on database sssions. So it will remove the need to rewrite application code. 

## Evaluation
|  Criteria  | Comment                         | Managed Dev DB (AWS RDS PostgreSQL) | Continuous Data Pulls from Kimble (e.g., API/Replication) |
| :--------: | ------------------------------- | :-----------: | :-----------: |
| 1. Data Fidelity and Production Parity | Focus: Trade-off between Service Fidelity (RDS is High) and Data Fidelity (Kimble is Highest). |       4       |       5       |
| 2. Developer Isolation and Data Resets | Isolation: RDS is good but slow to reset. Kimble is poor due to reliance on a shared, slow ETL pipeline. |       3       |       1       |
| 3. Maintenance and Operational Cost/Overhead | Cost: RDS is moderate (hourly cost). Kimble is the highest (ETL build/maintenance cost). |       3       |       1       |
|   Total    |                                |       10       |       7        |

## Decision
We will proceed with the AWS RDS Postgresql DB. Utilizing a dedicated, low-cost Managed Dev DB instance (AWS RDS PostgreSQL) for the development environment.

## Considerations on selected technology
Choosing AWS RDS PostgreSQL makes certain tasks easier and others more difficult:

* Easier: Scaling instance power and general DB maintenance (patching, backups) are now handled by AWS.

* More Difficult: Offline development is no longer possible, requiring network access to the AWS VPC.

Risks and Mitigation 

* Cost Risk: We mitigate the hourly cost by enforcing automated stop/start schedules and defining small instance types.

* Security: We secure the database by mandating encryption at rest and strictly controlling access via IAM and non-public security groups.

* Data Realism Risk: We accept using synthetic data but mitigate the risk by improving seed scripts and reserving a separate, specialized environment for tests requiring high-fidelity, real-world data.
