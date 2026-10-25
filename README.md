# 🦉 OSQL

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Linux-0a0c10?style=flat-square"/>
  <img src="https://img.shields.io/badge/Category-OWeb-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/Part%20of-OwlSec%20Toolkit-7b5ea7?style=flat-square"/>
  <img src="https://img.shields.io/badge/Version-1.0-cyan?style=flat-square"/>
</p>

> **OSQL** — Professional SQL Injection Scanner

---

> ⚠️ **AUTHORISED USE ONLY** — Use only on systems you own or have explicit written permission to test. Unauthorised use is illegal.

---

## 📌 Overview

OSQL is a SQL injection scanner for authorised security testing. It tests GET parameters, forms and HTTP headers, and supports error-based, time-based and blind detection techniques. Targets can be scanned individually or in batch, with JSON report output.

---

## ✨ Features

- GET parameter, form and HTTP header injection scanning
- Error-based, time-based and blind SQL injection detection
- Batch scanning of multiple target URLs from a file
- Session cookie support and HTTP/HTTPS proxy routing
- Configurable request delay, timeout and blind-delay threshold
- JSON report output, activity logging and an offline self-test

---

## ⚙️ Requirements

- **Linux**
- **No Python installation needed** — runs as a standalone executable

---

## 🚀 Usage

```bash
./OSQL -u "http://example.com/item?id=1"
./OSQL -f "http://example.com/login" -c "session=abc"
./OSQL -B targets.txt -o report.json
./OSQL --help
```

---

## 📦 Part of OwlSec Toolkit

This tool is part of the **OwlSec** suite — a collection of 300+ security and privacy tools.

🔗 [owlsec.org](https://owlsec.org)

---

## ©️ License

MIT License — © Khaled S. Haddad

*Tools are distributed as pre-built executables. Source code is proprietary.*
