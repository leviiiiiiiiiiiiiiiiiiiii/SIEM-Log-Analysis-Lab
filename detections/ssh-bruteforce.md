# SSH Brute Force Detection

**Objective:** flag repeated failed SSH authentication attempts from the same source IP within a 5-minute window.

```spl
index="test" sourcetype="linux_secure" "Failed password"
| rex "from (?<src_ip>\d{1,3}(?:\.\d{1,3}){3})"
| bin _time span=5m
| stats count by _time src_ip
| where count >= 5
```

A threshold match is suspicious activity requiring investigation; it is not by itself proof of successful compromise.
