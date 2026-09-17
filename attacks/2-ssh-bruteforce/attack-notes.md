# Attack 1: SSH Brute Force

## What I did
Used Hydra from Kali to brute-force SSH on the Ubuntu target with a custom
password list containing the real (weak) password mixed with decoys.

## What happened
Hydra found the correct password but kept testing others afterward,
since its parallel tasks (`-t 4`) don't share success state. Adding
`-f` fixed this, stopping the attack right after the first success.

## Detection
Forwarded target's auth.log to Splunk and built a timeline query
(see `detection-query.spl`) showing failed logins followed by one
success from the same source IP, a clear brute-force pattern.

## What I learned
- sshd cuts off connections after a few failed attempts, which shows
  up as its own detection signal in the logs.
- Hydra's parallel tasks don't share state so use `-f` for a clean run.
- Splunk's `rex` command is key for pulling fields out of raw logs.