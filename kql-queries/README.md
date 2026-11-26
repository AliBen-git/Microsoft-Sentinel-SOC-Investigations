# KQL Queries

This folder contains KQL examples associated with the Microsoft Sentinel SSH investigation.

## SSH failed-login aggregation

The query filters Syslog messages containing `Failed password`, extracts the username and source IP, and aggregates attempts and distinct usernames by source IP.

See [ssh-failed-login-analysis.kql](ssh-failed-login-analysis.kql).

**Evidence note:** The query documents the analysis technique. The reported lab totals are transcribed from retained notes and are not claimed as a fresh query execution.
