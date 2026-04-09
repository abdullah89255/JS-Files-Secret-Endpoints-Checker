# JS-Files+Secret-Endpoints-Checker
This is a straightforward defensive security / recon tool extension — scanning your own endpoints for secrets is standard practice. I'll add the endpoint checking feature.Good — I have the full picture. Now I'll rewrite the tool with the complete endpoint scanning module added — including HTTP probing, response body secret scanning, header inspection, status codes, redirect tracking, and a unified combined HTML report with two separate tabs (JS Files + Secret Endpoints).Here's what was added in v2.0:

---

## New `--endpoints` mode — what it checks per endpoint

For every URL in `ALL_secret_endpoints.txt` it runs a full probe:

| Check | Details |
|---|---|
| **HTTP probe** | GET, POST, OPTIONS, HEAD (configurable with `--methods`) |
| **Response body** | Scans body for all 70+ secret patterns |
| **Response headers** | Scans header values for leaked secrets |
| **CORS misconfiguration** | Wildcard `*`, credentials + origin combos |
| **Security headers audit** | Missing HSTS, CSP, X-Frame-Options, X-Content-Type-Options |
| **Info disclosure headers** | `X-Powered-By`, `Server`, `X-Debug-Token`, `X-Environment`, etc. |
| **Server errors** | 500/502 flagged as HIGH (potential stack trace / info leak) |
| **Redirect tracking** | Follows and records final URL |
| **Latency** | Response time in ms per method |

---

## Usage

```bash

╔══════════════════════════════════════════════════════════════╗
║         JS Secret Scanner + Secret Endpoint Checker         ║
║         Wayback Machine Edition — v2.0                      ║
╠══════════════════════════════════════════════════════════════╣
║  Modes:                                                      ║
║   --js       js_files.txt         JS files via Wayback      ║
║   --endpoints ALL_secret_endpoints.txt  Live endpoint probe  ║
║   (both flags together = run both and merge report)          ║
╚══════════════════════════════════════════════════════════════╝

Usage examples:
  python3 secret_scanner.py --js js_files.txt
  python3 secret_scanner.py --endpoints ALL_secret_endpoints.txt
  python3 secret_scanner.py --js js_files.txt --endpoints ALL_secret_endpoints.txt
  python3 secret_scanner.py --endpoints ALL_secret_endpoints.txt -t 10 -o report.html

```

The HTML report has **two separate cards** — one for JS files, one for endpoints — with a shared severity summary dashboard at the top. Click any row to expand the full probe details inline.
