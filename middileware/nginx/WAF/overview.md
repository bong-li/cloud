# WAF

[toc]

### 概述

#### 1.WAF
web application firewall

#### 2.ModSecurity
是一个动态库，nginx可以加载相应的模块，从而使用ModSecurity，从而能够设置各种规则，从而能够成为WAF

#### 2.OWASP CRS
open web application security project core rule set
为NGINX ModSecurity WAF提供规则，用于阻止相关攻击：
* SQL Injection (SQLi)
* cross‑site scripting (XSS)
* Local File Inclusion (LFI)
* Remote File Inclusion (RFI)
* PHP Code Injection
* Java Code Injection
* Unix/Windows Shell Injection
* Scripting/Scanner/Bot Detection
* Session Fixation
* Metadata/Error Leakages
