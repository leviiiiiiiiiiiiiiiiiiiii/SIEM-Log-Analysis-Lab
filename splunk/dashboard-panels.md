# Dashboard Panels

## Linux Events — Single Value
```spl
index="test" sourcetype="linux_secure" | stats count
```

## Web Events — Single Value
```spl
index="test" sourcetype="access_combined_wcookie" | stats count
```

## Top Web Client IPs — Bar Chart
```spl
index="test" sourcetype="access_combined_wcookie" | stats count by clientip | sort - count | head 10
```

## Failed SSH Attempts — Bar Chart
```spl
index="test" sourcetype="linux_secure" "Failed password" | rex "from (?<src_ip>\d{1,3}(?:\.\d{1,3}){3})" | stats count by src_ip | sort - count | head 10
```

## SSH Failures Over Time — Line Chart
```spl
index="test" sourcetype="linux_secure" "Failed password" | timechart span=5m count
```

## HTTP Status Codes — Column Chart
```spl
index="test" sourcetype="access_combined_wcookie" | stats count by status | sort - count
```

## Web 404 Requests — Bar Chart
```spl
index="test" sourcetype="access_combined_wcookie" status=404 | stats count by clientip | sort - count | head 10
```

## Possible Web Scanning — Table
```spl
index="test" sourcetype="access_combined_wcookie" status=404 | stats count dc(uri) as unique_urls by clientip | where count > 20 | sort - count
```
