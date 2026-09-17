# Attack 3: Privilege Escalation

## What I did
Created a low-privilege user (guest) with no sudo access, then granted
it passwordless sudo access to a single command (/usr/bin/find) through a
deliberately misconfigured sudoers rule. This recreates a common real-world
misconfiguration. This scenario assumes the attacker already gained
low-privilege access (the brute-force attack in Attack 2)
and is now escalating from that initial access.

## What happened
Used a known GTFOBins technique to abuse find's -exec flag, spawning a
root shell as guest: `sudo find . -exec /bin/bash \;`. Confirmed the
escalation succeeded by checking `whoami`, which returned root.

## Detection
Installed auditd and added watch rules for sudo execution. The audit
trail showed sessions where AUID (original login user) was "guest" but
UID became "root" which is proof of escalation, since AUID persists even after
privileges change. Built a Splunk query generalizing this pattern to
flag any user escalating to root, not just guest specifically.

## What I learned
- AUID (audit user ID) is immutable once set at login, even if a user
  escalates privileges which is exactly why it's used for accountability
  in audit trails, unlike UID which changes with privilege escalation.
- find -exec spawns a new shell for every matched file rather than
  stopping after the first, which is why `exit` didn't immediately
  return to a normal prompt. GTFOBins' documented payload includes a
  -quit flag specifically to avoid this behavior.
- Writing detection logic around the underlying pattern (AUID != UID
  root mismatch) rather than a specific username makes the rule
  reusable for any future escalation, not just this one test case.