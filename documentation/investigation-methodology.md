# Investigation Methodology

## 1. Data source

The lab investigation used Syslog data collected from an Azure-hosted Linux VM and queried in Microsoft Sentinel / Log Analytics.

## 2. Initial filtering

Failed SSH password messages were identified by filtering Syslog message text for:

```kusto
SyslogMessage contains "Failed password"
```

## 3. Field extraction

The documented KQL parsing approach extracted the account name and source IP from the message text:

```kusto
| parse SyslogMessage with * "for " User " from " SrcIP " port " *
```

## 4. Aggregation

The investigation counted events and distinct usernames per source IP:

```kusto
| summarize Attempts = count(), UsersTargeted = dcount(User) by SrcIP
| order by Attempts desc
```

## 5. Recorded analysis

The retained notes record three sources with 30, 37, and 22 failed attempts respectively. They also record the number or identity of usernames targeted for each source.

## 6. Hunting bookmark

A hunting bookmark was created for `138.124.7.7`.

## 7. Assessment discipline

Failed authentication events indicate attempts, not successful access. A successful login must be reviewed and correlated with sufficient context before it can be assessed as unauthorized. In this case, the retained material does not verify a successful SSH login or a compromise.

## 8. Reproducibility limitation

The original lab is no longer available. The methodology and query are documented from retained material; no claim is made that the query was rerun or that raw events were revalidated during repository preparation.
