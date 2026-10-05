# Brute-Force Detection Investigation — Rule 100070



## Objective

My aim was to create a custom Wazuh rule to detect repeated Windows logon failures against the same account.



## Investigation

I generated multiple failed Windows logon attempts against the same account.

Windows generated Event ID 4625 for the failed logons.

I initially investigated Rule 60105 as the possible parent rule, but the resulting Wazuh alert was using Rule 60122.

So i used Rule 60122 as the parent for the custom detection.



## Detection Development

I created Rule 100070 using Rule 60122 as the matched rule.

The rule was configured to trigger after 5 failed logon events against the same username within 60 seconds.



## Testing

I tested the rule by generating five incorrect password attempts against the same Windows account within 60 seconds.

The failed logon events were collected by Wazuh and Rule 100070 successfully generated a Level 10 alert.



## Result

The custom brute-force detection worked successfully and was mapped to MITRE ATT\&CK T1110 — Brute Force.

The investigation also helped me understand how to identify the actual Wazuh rule responsible for a Windows authentication event instead of relying only on the expected rule hierarchy.

