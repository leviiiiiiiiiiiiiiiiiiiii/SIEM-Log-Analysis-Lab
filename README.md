# SIEM Log Analysis Lab

A practical Splunk-based SOC analyst lab covering log ingestion, search, detection, investigation, alerting, and dashboarding using Web Application and Linux security logs.

## Environment
- Splunk Enterprise 10.4.1
- Index: `test`
- Web sourcetype: `access_combined_wcookie`
- Linux sourcetype: `linux_secure`

## Workflow
Log Ingestion → Search → Field Extraction → Detection → Investigation → Alert → Dashboard → Documentation

## Evidence
The `screenshots/` folder contains the supplied Splunk evidence plus a final dashboard visual mockup.

## Key findings visible in the supplied evidence
- Web events: 131,645
- Linux events: 63,884
- Top displayed Web client IP: `87.194.216.51` with 3,316 requests
- HTTP status results include 200, 400, 403, 404, 406, 408, 500, 503, and 505
- 404 analysis shows several client IPs generating more than 20 failed requests/unique URLs
- Linux logs contain repeated `Failed password` SSH events

## SSH brute-force detection
```spl
index="test" sourcetype="linux_secure" "Failed password"
| rex "from (?<src_ip>\d{1,3}(?:\.\d{1,3}){3})"
| bin _time span=5m
| stats count by _time src_ip
| where count >= 5
```

## Investigation
```spl
index="test" sourcetype="linux_secure"
| rex "from (?<src_ip>\d{1,3}(?:\.\d{1,3}){3})"
| search src_ip="87.194.216.51"
| table _time src_ip host _raw
| sort _time
```

## Alert
Alert: `SSH Brute Force Detection`
- Type: Scheduled
- Trigger condition: Number of Results > 0
- Trigger: Once
- Action: Add to Triggered Alerts

## Dashboard
`11-final-dashboard-mockup.png` is a visual mockup generated from values visible in the supplied screenshots. It is **not a direct screenshot from Splunk**. For a public portfolio, label it as a mockup unless you recreate the dashboard in Splunk and capture it there.

## Repository structure
```text
SIEM-Log-Analysis-Lab/
├── README.md
├── screenshots/
├── queries/
├── detections/
├── reports/
└── splunk/
```


