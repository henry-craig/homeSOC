# Attack 2: Nmap Port Scan

## What I did
Ran an Nmap SYN scan (`-sS`) from Kali against Ubuntu target for
the 1000 most common TCP ports.

## What happened
Only port 22 (SSH) responded as open; the remaining 999 ports were
filtered by UFW, which was configured to allow only SSH. UFW blocked
and logged the scan traffic to `/var/log/ufw.log`.

## Detection
Forwarded `ufw.log` to Splunk and wrote a query counting distinct
destination ports per source IP. Initial threshold (`>10`) was set
before observing real log volume but UFW's logging rate-limits blocked
entries, so only ~9 of the ~1000 scanned ports were actually logged.
Adjusted the threshold to `>5` to match realistic log volume.

## What I learned
- UFW logs "filtered" rather than "closed" for blocked ports, so Nmap
  can't distinguish a firewall block from a genuinely closed port.
- UFW rate-limits logging (especially at low/medium levels) to
  prevent log flooding meaning full port-scan coverage in logs is
  unrealistic, and detection thresholds should be calibrated to real
  observed volume, not assumed coverage.
- Clearing the Splunk index between test runs is a must to avoid
  overlapping data from separate scans messing with detection counts.