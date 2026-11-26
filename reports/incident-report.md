# SIEM Incident Analysis Report

## Scenario
Suspected SSH brute-force / repeated authentication-failure activity.

## Detection
Repeated `Failed password` events are bucketed into 5-minute windows and flagged when a source IP reaches 5 or more failures.

## Investigation
The investigator searches all Linux authentication events associated with a selected source IP and reviews `_time`, `src_ip`, `host`, and `_raw` to build a timeline.

## Findings
The supplied screenshots show repeated SSH `Failed password` messages from multiple source IPs. The investigated IP `87.194.216.51` appears repeatedly in the supplied event evidence.

## Conclusion
The evidence supports a suspicious authentication pattern that warrants investigation. It should not be described as a confirmed compromise without additional corroborating evidence.
