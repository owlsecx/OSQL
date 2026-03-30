# 🦉 OSQL

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Linux%20%2F%20Windows-informational?style=flat-square&logo=linux&logoColor=white&color=0a0c10"/>
  <img src="https://img.shields.io/badge/Category-OWeb%20%2F%20Injection-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/No%20Dependencies-stdlib%20only-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/Part%20of-OwlSec%20Toolkit-7b5ea7?style=flat-square"/>
  <img src="https://img.shields.io/badge/Version-1.0-cyan?style=flat-square"/>
</p>

> **OSQL** is a SQL injection scanner with GET/POST parameter testing, boolean and time-based blind detection, database fingerprinting, WAF detection, batch scanning, and JSON export — no external dependencies required.

---

> ⚠️ **AUTHORISED SECURITY TESTING ONLY** — Use only on systems you own or have explicit written consent to test.

---

## 📌 Overview

OSQL tests web application parameters for SQL injection using three detection techniques — error-based, boolean-blind, and time-based blind — against 7 database engines and 8 WAF signatures. It can be used interactively from the menu or non-interactively via CLI flags.

---

## 🖥️ Menu

| Option | Description |
|--------|-------------|
| **[1] URL Scanner** | Parse all GET parameters from the URL and inject error-based + union payloads |
| **[2] Form Scanner** | Fetch the page, parse all HTML forms, inject each field via POST |
| **[3] Blind SQLi** | Boolean-based and time-based blind detection per parameter |
| **[4] Error Analyser** | Database fingerprinting from error messages in the page source |
| **[5] Batch Scanner** | Test multiple URLs from a file — quick GET param check per target |
| **[E] Export Session** | Save all scans from the current session to a single JSON file |

---

## 🔍 Detection Techniques

### Error-Based
Injects 30+ payloads including `'`, `''`, `OR 1=1`, `UNION SELECT`, `ORDER BY`, `EXTRACTVALUE`, and stacked query variants. Matches error patterns in the response body against 80+ database-specific signatures.

### Boolean-Blind
Tests 9 true/false payload pairs per parameter and compares response lengths. A significant difference (> 50 bytes) between the true and false responses flags a possible blind injection.

| True Payload | False Payload |
|-------------|--------------|
| `' AND '1'='1` | `' AND '1'='2` |
| `' AND 1=1--` | `' AND 1=2--` |
| `1 AND 1=1` | `1 AND 1=2` |
| + 6 more pairs | |

### Time-Based Blind
Injects 12 sleep payloads and measures response time. A delay exceeding the configured threshold (default 4s) flags a positive.

| Payload | Database |
|---------|----------|
| `' AND SLEEP(5)--` | MySQL |
| `'; WAITFOR DELAY '0:0:5'--` | MSSQL |
| `'; SELECT pg_sleep(5)--` | PostgreSQL |
| `1 AND 1=DBMS_PIPE.RECEIVE_MESSAGE('a',5)--` | Oracle |
| + 8 more | |

---

## 🗄️ Database Fingerprinting

Detects 7 database engines using 80+ error signatures matched against response body:

| Database | Example Signatures |
|----------|--------------------|
| **MySQL** | `you have an error in your sql syntax`, `com.mysql.jdbc` |
| **MSSQL** | `microsoft sql server`, `incorrect syntax near`, `unclosed quotation mark` |
| **Oracle** | `ORA-\d{5}`, `quoted string not properly terminated`, `PL/SQL` |
| **PostgreSQL** | `syntax error at or near`, `unterminated quoted string`, `npgsql` |
| **SQLite** | `sqlite error`, `near "": syntax error` |
| **DB2** | `db2 sql error`, `SQLCODE=-` |
| **Sybase** | `sybase message`, `com.sybase.jdbc` |

---

## 🛡️ WAF Detection

Detects 8 WAF vendors from response headers and body:

`Cloudflare` · `ModSecurity` · `AWS WAF` · `Akamai` · `Incapsula` · `F5 BIG-IP` · `Barracuda` · `Sucuri`

Also flags HTTP `403`, `406`, `429` responses as `Unknown WAF (blocked)`.

---

## 💻 CLI Mode

OSQL supports non-interactive command-line usage:

```bash
# GET parameter scan
./OSQL -u "http://target.com/page.php?id=1"

# POST form scan
./OSQL -f "http://target.com/login.php"

# Blind scan with custom threshold
./OSQL -b "http://target.com/page.php?id=1" -T 5

# Error analysis
./OSQL -e "http://target.com/page.php?id=1"

# Batch scan from file
./OSQL -B targets.txt -d 0.5

# With cookie and JSON export
./OSQL -u "http://target.com/page.php?id=1" -c "session=abc123" -o report.json
```

### CLI Flags

| Flag | Description | Default |
|------|-------------|---------|
| `-u` | URL for GET parameter scan | — |
| `-f` | URL for form scan | — |
| `-b` | URL for blind scan | — |
| `-e` | URL for error analysis | — |
| `-B` | File with one URL per line (batch) | — |
| `-c` | Session cookie | — |
| `-d` | Delay between requests (seconds) | `0.3` |
| `-t` | Request timeout (seconds) | `10` |
| `-T` | Time-based detection threshold (seconds) | `4.0` |
| `-o` | Save JSON report to file | — |

---

## 📤 Reports

Each scan offers a JSON export saved as:

```
osql_<type>_YYYYMMDD_HHMMSS.json
```

Types: `url` · `form` · `blind` · `error` · `batch` · `session`

Each report contains the target URL, parameters tested, findings (type, parameter, payload, database, evidence), WAF status, and total test count.

---

## ⚙️ Requirements

- **Linux or Windows**
- **No Python installation needed** — runs as a standalone executable
- **No external dependencies** — uses Python standard library only

---

## 🚀 Usage

```bash
# Interactive menu
./OSQL

# Direct scan
./OSQL -u "http://target.com/page.php?id=1"
```

---

## 📦 Part of OwlSec Toolkit

This tool is part of the **OwlSec** suite — a collection of 300+ security and privacy tools.

🔗 [owlsec.org](https://owlsec.org)

---

## ©️ License

MIT License — © Khaled S. Haddad

*Tools are distributed as pre-built executables. Source code is proprietary.*
