# Incident Report: SSH Brute Force Activity

## Executive summary

A review of retained Microsoft Sentinel lab material identified 89 failed SSH authentication attempts from three source IP addresses. The activity included repeated attempts against the `root` account and attempts involving multiple usernames. A hunting bookmark was created for `138.124.7.7`.

The available material does not verify a successful SSH login. Unauthorized access and system compromise are therefore **not confirmed**.

## Scope and evidence basis

This report is based on lab screenshots, KQL examples, notes, and module documentation retained from April 2026. The Azure lab is no longer accessible, so the original queries and raw events could not be re-run or independently revalidated.

## Findings

| Source IP | Failed attempts | Recorded target detail |
|---|---:|---|
| `138.124.7.7` | 30 | `root` |
| `87.106.91.226` | 37 | 14 usernames |
| `192.252.215.125` | 22 | 15 usernames |
| **Total** | **89** | — |

### Behavioral observations

- Repeated failed SSH authentication attempts were recorded.
- Multiple usernames were targeted by two of the listed sources.
- Activity against `root` was recorded for `138.124.7.7`.
- A hunting bookmark was created for `138.124.7.7`.

### Assessment

The observed pattern is consistent with SSH brute-force activity and username enumeration. This is an assessment of the recorded failed-authentication pattern, not attribution of the activity to a named actor.

## Successful authentication and compromise

A successful SSH login is **not verified in the available material**. The retained evidence does not establish whether any successful login was associated with these source IPs. No unauthorized access, persistence, or host compromise can be concluded from the evidence currently available.

## Actions documented

- Syslog data was examined using KQL.
- Usernames and source IP addresses were parsed from failed-login messages.
- Attempts and targeted usernames were aggregated by source IP.
- A hunting bookmark was created for `138.124.7.7`.

## Evidence limitations

- The original Azure environment is no longer available.
- Raw event exports and complete query results are not present in the retained material.
- Exact event timestamps and session-level correlation are not documented in the current evidence set.
- The bookmark is documented, but its contents and subsequent disposition are not available here.

## Conclusion

The lab investigation identified 89 failed SSH login attempts across three source IP addresses, with patterns consistent with brute force and username enumeration. A bookmark was created for one source. A successful unauthorized login or compromise was not verified; the conclusion is limited to the evidence retained from the lab.
