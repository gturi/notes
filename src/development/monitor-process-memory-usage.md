---
tags:
  - development
  - linux
  - bash
  - monitoring
  - memory-usage
---

# Monitor process memory usage

```bash
#!/bin/bash

# Usage: ./monitor-memory.sh <PID> [interval_seconds]
# Example: ./monitor-memory.sh 12345 5

PID=$1
INTERVAL=${2:-1}

if [ -z "$PID" ] || ! ps -p "$PID" > /dev/null; then
  echo "Usage: $0 <PID> [interval_seconds]"
  exit 1
fi

min=9999999999999
max=0
sum=0
count=0

echo "Monitoring memory usage for PID $PID every $INTERVAL second(s)..."
echo "Press Ctrl+C to stop."

trap 'echo; echo "Final stats: min=${min}MB max=${max}MB avg=$((sum/count))MB"; exit 0' SIGINT

while true; do
  if ! ps -p "$PID" > /dev/null; then
    echo "Process $PID ended."
    break
  fi

  # Get RSS in KB and convert to MB
  mem_kb=$(grep VmRSS "/proc/$PID/status" | awk '{print $2}')
  mem_mb=$((mem_kb / 1024))

  # Handle potential race condition where proc file isn't available yet
  if [ -z "$mem_mb" ]; then
    sleep "$INTERVAL"
    continue
  fi

  (( mem_mb < min )) && min=$mem_mb
  (( mem_mb > max )) && max=$mem_mb
  (( sum += mem_mb ))
  (( count++ ))

  echo "Current: ${mem_mb}MB | Min: ${min}MB | Max: ${max}MB | Avg: $((sum/count))MB"

  sleep "$INTERVAL"
done
```


## Start a java application and get its pid

```bash
#!/bin/bash

appName="my-application"

exec -a "$appName" \
   java -jar my-app.jar &

appPid="$(pidof "$appName")"

sleep 1

echo "Application pid '$appPid'"
```
