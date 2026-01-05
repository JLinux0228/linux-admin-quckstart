#!/bin/bash

echo "=== System Health Check ==="
echo "Hostname: $(hostname)"
echo ""

# Uptime (Linux vs macOS)
if uptime -p &>/dev/null; then
  echo "Uptime:"
  uptime -p
else
  echo "Uptime:"
  uptime
fi

echo ""
echo "Disk Usage:"
df -h /

echo ""
echo "Memory:"
if command -v free &>/dev/null; then
  free -h
else
  vm_stat | awk '
    /Pages free/ { free=$3 }
    /Pages active/ { active=$3 }
    /Pages inactive/ { inactive=$3 }
    END {
      print "Free Pages:", free
      print "Active Pages:", active
      print "Inactive Pages:", inactive
    }'
fi

echo ""
echo "Top processes (by memory):"
if ps aux --sort=-%mem &>/dev/null; then
  ps aux --sort=-%mem | head -n 6
else
  ps aux | sort -nrk 4 | head -n 6
fi


## Roadmap
- [x] Initial project structure
- [x] System health check script
- [ ] User onboarding automation
- [ ] Backup and log rotation script
- [ ] Firewall and SELinux references
