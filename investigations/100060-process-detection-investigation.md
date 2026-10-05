\# Suspicious Process Detection Investigation — Rule 100060



\## Objective

My aim was to create a custom Wazuh rule to detect the creation of commonly used Windows command and scripting interpreters.



\## Investigation

I enabled Process Creation auditing on the Windows VM so that Windows would generate process creation events.

Windows generated Event ID 4688 when a new process was created.

I investigated the Wazuh alert and identified Rule 67027 as the existing rule detecting process creation events.

I initially created the custom rule using a regular expression, but it did not trigger during testing.

I investigated the rule and found that the field needed to use the PCRE2 regex type, this is because Wazuh was treating the field condition as its default field-matching pattern, rather than interpreting it with PCRE2 regex engine.



\## Detection Development

I updated Rule 100060 to use type="pcre2" and configured it to detect PowerShell, CMD, WScript, CScript and MSHTA.

The rule was configured to generate a Level 10 alert when one of these processes was created.



\## Testing

I tested the rule by launching PowerShell, CMD, WScript, CScript and MSHTA on the Windows VM.

Rule 100060 successfully generated a Level 10 alert for each tested process.



\## Result

The custom process detection worked successfully and was mapped to MITRE ATT\&CK T1059 — Command and Scripting Interpreter.

The investigation also helped me understand how Wazuh processes Windows Event ID 4688 and how PCRE2 can be used for more specific detection matching.

