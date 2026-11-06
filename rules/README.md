# قواعد Snort المخصصة · Custom Snort Rules

أربع عشرة قاعدة كشف مخصّصة كُتبت داخل المختبر لتغطية السيناريوهات الخمسة.
الجدول أدناه يوثّق غرض كل قاعدة ورقمها. ملف القواعد نفسه يُنشر بعد تنقيح العناوين الداخلية.

Fourteen custom detection rules written inside the lab to cover the five attack
scenarios. The table below documents each rule's purpose and SID.

## القواعد · Rules

| SID | الغرض |
|---|---|
| 1000001 | ICMP from UNTRUST — Ping Scan |
| 1000002 | Nmap SYN Scan |
| 1000003 | Nmap XMAS Scan |
| 1000004 | Nmap NULL Scan |
| 1000005 | RDP Brute Force |
| 1000006 | SMB from UNTRUST — Lateral Movement |
| 1000007 | SSH Brute Force |
| 1000008 | EICAR Test |
| 1000009 | DNS to C2 |
| 1000010 | Data Exfiltration |
| 1000011 | Splunk from non-mgmt |
| 1000012 | Wazuh from non-mgmt |
| 1000013 | Telnet cleartext |
| 1000014 | FTP cleartext |

> تُستخدم متغيرات `$HOME_NET` و`$EXTERNAL_NET` بدل العناوين الصريحة،
> حتى تبقى القواعد قابلة لإعادة الاستخدام في أي مختبر.
> Rules use `$HOME_NET` / `$EXTERNAL_NET` variables rather than hardcoded addresses,
> keeping them portable across labs.
