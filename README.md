<div align="center">

<img src="docs/social-preview.png" width="860" alt="LAYERS — SOC Lab">

### طبقات · LAYERS — EP. 01

# SOC Lab — Defense in Depth
## مختبر مركز عمليات أمنية متعدد الطبقات

**بنينا شركة وهمية. ثم هاجمناها.**
*We built a fake company. Then we attacked it.*

![VMware](https://img.shields.io/badge/VMware-607078?style=flat-square&logo=vmware&logoColor=white)
![pfSense](https://img.shields.io/badge/pfSense-212121?style=flat-square&logo=pfsense&logoColor=white)
![Snort](https://img.shields.io/badge/Snort-EF3B2D?style=flat-square)
![Splunk](https://img.shields.io/badge/Splunk-000000?style=flat-square&logo=splunk&logoColor=white)
![Wazuh](https://img.shields.io/badge/Wazuh-3585F9?style=flat-square)
![MITRE](https://img.shields.io/badge/MITRE%20ATT%26CK-E0A03C?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

</div>

---

## ما هذا؟ · What is this?

مختبر متكامل لمركز عمليات أمنية داخل VMware يحاكي شبكة شركة حقيقية:
**6 أجهزة** موزّعة على **3 شبكات مفصولة**، محمية بـ **7 طبقات دفاع**،
اختبرناها بـ **5 سيناريوهات هجوم** موثّقة.

A complete SOC lab inside VMware simulating a real corporate network:
**6 hosts** across **3 segmented networks**, protected by **7 defense layers**,
tested with **5 documented attack scenarios**.

> مشروع جماعي ضمن فريق من أربعة · A team project by four members

---

## المخطط · Architecture

<div align="center">
<img src="docs/network-diagram.png" width="820" alt="Lab network diagram">
</div>

| الشبكة | النوع | الغرض |
|---|---|---|
| `WAN` | NAT | الإنترنت — ومنها تُنفّذ الهجمات |
| `LAN` | Host-only | شبكة الشركة: DC، SIEM، HIDS، محطة عمل |
| `UNTRUST` | Host-only | شبكة المهاجم المعزولة |

---

## طبقات الدفاع · Defense Layers

| # | الطبقة | الأدوات |
|---|---|---|
| 1 | المحيط · Perimeter | pfSense + Snort |
| 2 | الهوية · Identity | Active Directory + Group Policy |
| 3 | الجهاز · Endpoint | AppLocker · USB restrictions · Sysmon |
| 4 | الإدارة · Management | Endpoint Central |
| 5 | التدقيق · Audit | ADAudit Plus |
| 6 | الرؤية · Visibility | Splunk + Wazuh |
| 7 | الاستجابة · Response | Auto-block + GPO + Wazuh Active Response |

---

## قواعد الكشف · Detection Rules

**14 قاعدة مخصصة** فوق أكثر من 10,000 قاعدة جاهزة (Talos VRT · GPLv2 · ET Open · OpenAppID).

| SID | الكشف |
|---|---|
| `1000001` | ICMP / Ping Scan |
| `1000002-4` | Nmap SYN · XMAS · NULL |
| `1000005` | RDP Brute Force |
| `1000006` | SMB Lateral Movement |
| `1000007` | SSH Brute Force |
| `1000009` | DNS to C2 |
| `1000010` | Data Exfiltration |
| `1000013-14` | Telnet · FTP cleartext |

القواعد كاملة في [`rules/local.rules`](rules/local.rules)

---

## سيناريوهات الاختبار · Attack Scenarios

| # | السيناريو | كيف كُشف |
|---|---|---|
| 1 | فحص منافذ SYN | Snort → Splunk dashboard |
| 2 | قوة غاشمة على RDP | Snort + Event `4625` + ADAudit alert |
| 3 | نسخ بيانات إلى USB | GPO block + Event `4663` |
| 4 | تطبيق غير مصرّح به | AppLocker + Event `8004` + Wazuh |
| 5 | تصعيد صلاحيات | Event `4728` + ADAudit critical alert |

**النتيجة:** أكثر من 20 نوع تنبيه، موثّقة وفق MITRE ATT&CK.

---

## الدرس · The Lesson

> الأدوات وحدها لا تصنع مركز عمليات أمنية — الربط بينها هو ما يصنعه.
> *Tools alone don't make a SOC. Connecting them does.*

جدار الحماية وحده لا يرى الداخل. وSIEM وحده لا يمنع. ومراقبة الجهاز وحدها لا تفهم الشبكة.

---

## محتويات المستودع · Repository Contents

```
├── docs/
│   ├── network-diagram.png
│   ├── report.pdf              ← التقرير الكامل (33 صفحة)
│   └── screenshots/            ← لقطات حقيقية من المختبر
├── rules/
│   └── local.rules             ← 14 قاعدة Snort مخصصة
├── configs/
│   ├── gpo/                    ← سياسات المجموعة السبع
│   └── splunk/                 ← تعريفات الفهارس
└── scenarios/                  ← خطوات كل سيناريو ونتيجته
```

---

## إخلاء مسؤولية · Disclaimer

كل الاختبارات نُفّذت داخل **بيئة مختبرية معزولة ومصرّح بها** بالكامل.
لا أنظمة حقيقية، لا بيانات لأحد، لا عناوين عامة.

All testing was performed in a **fully isolated, authorized lab environment**.
No real systems. No third-party data. No public targets.

---

## الفريق · Team

مشروع جماعي من أربعة أعضاء، بإشراف د. نور الدين الجراتلي.
*A four-member team project, supervised by Dr. Noureddine Al-Jaradi.*

---

<div align="center">

**[السلسلة على LinkedIn ←](https://www.linkedin.com/in/ammar-almosbahi-963706381/)**

MIT License

</div>
