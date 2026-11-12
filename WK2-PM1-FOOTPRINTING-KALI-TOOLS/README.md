# W2-PM1: Footprinting & Reconnaissance with Multiple Kali Tools

**Program:** Networkwalks Cybersecurity & Ethical Hacking | **Batch:** B084D | **Week:** 2
**Author:** Morakinyo | **Date:** 11 November 2026 | **Environment:** Kali Linux (VM)

> **For educational purposes only.** All activity was performed on a lab-designated target (networkwalks.com, the training provider's site). No exploitation or intrusive testing was carried out. Only test systems you own or have written permission to test.

## 1. Objective

Footprint the live website `networkwalks.com` using six built-in Kali Linux tools to build a profile of the target from public information: registration details, web technologies, IP address, HTTP headers, firewall protection and DNS records.

## 2. Tools Used

| Tool | Purpose |
|---|---|
| `whois` | Find domain registration details (registrar, dates, name servers) |
| `whatweb` | Fingerprint web technologies (server, CMS, plugins, IP) |
| `nslookup` | Resolve the domain name to its IP address |
| `curl -I` | Read the HTTP response headers |
| `wafw00f` | Detect whether a Web Application Firewall protects the site |
| `dnsrecon` | Enumerate DNS records (NS, MX, SPF, TXT, SRV) |

## 3. Activities and Findings

### Task 1: Domain registration (whois)

**Command:** `whois networkwalks.com`

![whois output, part 1](screenshots/01-whois-1.jpg)
![whois output, part 2](screenshots/02-whois-2.jpg)

| Field | Result |
|---|---|
| Registrar | GoDaddy.com, LLC (IANA ID 146) |
| Created | 2019-11-06 |
| Last updated | 2025-11-12 |
| Expires | 2027-11-06 |
| Name servers | NS6135.HOSTGATOR.COM, NS6136.HOSTGATOR.COM |
| DNSSEC | Unsigned |
| Status | clientTransfer, clientUpdate, clientRenew and clientDelete Prohibited |
| Registrant | Hidden behind a privacy proxy (Domains By Proxy) |

**Why it matters:** The name servers identify HostGator as the hosting provider. Registration dates and abuse contacts can support social engineering. The privacy proxy and the transfer/delete locks are good practice.

### Task 2: Web technology fingerprint (whatweb)

**Command:** `whatweb networkwalks.com`

![whatweb output](screenshots/03-whatweb.jpg)

- `http://` returned **301 Moved Permanently** and redirected to `https://networkwalks.com/`.
- Over HTTPS the site returned **200 OK**.
- Detected: **Apache**, **WordPress 7.1.4**, **WordPress Download Manager 3.3.76**, jQuery 3.7.1, Bootstrap, HTML5, Google Tag Manager and Open Graph protocol.
- Page title: Networkwalks Academy. Server IP: **192.232.216.135** (United States).
- A public contact email (`info@networkwalks.com`) was exposed.

**Why it matters:** Exact software versions can be searched in vulnerability databases for known issues, which is why patching and removing version tags matter.

### Task 3: DNS resolution (nslookup)

**Command:** `nslookup networkwalks.com`

![nslookup output](screenshots/04-nslookup.jpg)

- The domain resolved to **192.232.216.135** (non-authoritative answer from resolver 172.20.10.1).
- The tool then printed a communications timeout from the resolver. This was a local DNS issue and not a finding about the target.

**Why it matters:** Knowing the IP lets an attacker scan the server directly and look for other sites sharing the same address.

### Task 4: HTTP response headers (curl -I)

**Command:** `curl -I http://networkwalks.com`

![curl output](screenshots/05-curl.jpg)

| Header | Observation |
|---|---|
| Status | 301 Moved Permanently, redirecting to `https://networkwalks.com/` |
| Server | Apache |
| X-Redirect-By | WordPress - Really Simple Security |
| Cache headers | X-nginx-cache: WordPress, X-Endurance-Cache-Level: 0 |
| Cookie | `__wpdm_client` set with HttpOnly |
| Security-related headers | Permissions-Policy and Referrer-Policy present |

**Why it matters:** Headers reveal the web server, the caching layer and the security plugin in use without loading the page. On the positive side, the site forces HTTPS and sets some protective headers.

### Task 5: WAF detection (wafw00f)

**Command:** `wafw00f networkwalks.com`

![wafw00f output](screenshots/06-wafw00f.jpg)

- Result: the site is behind **ModSecurity (SpiderLabs)** WAF, detected in 2 requests.

**Why it matters:** Knowing a WAF is present changes how an attacker proceeds, because naive attempts get blocked or logged. For defenders, it confirms the WAF is active.

### Task 6: DNS enumeration (dnsrecon)

**Command:** `dnsrecon -d networkwalks.com`

![dnsrecon output](screenshots/07-dnsrecon.jpg)

| Record | Value |
|---|---|
| SOA | ns6135.hostgator.com (50.87.144.87) |
| NS | ns6135.hostgator.com (50.87.144.87), ns6136.hostgator.com (192.232.216.131) |
| BIND version | "9.16.23-RH" disclosed by the name servers |
| MX | mail.networkwalks.com (192.232.216.135) |
| A | networkwalks.com (192.232.216.135) |
| TXT (SPF) | `v=spf1 +a +mx +ip4:50.87.144.87 +include:websitewelcome.com ~all` |
| TXT | Google site verification record |
| SRV | 8 `_autodiscover._tcp` records pointing to cpanelemaildiscovery.cpanel.net (port 443) |
| DNSSEC | No answer to the DNSSEC query |

The first run hit resolver timeouts and was repeated successfully. 8 records were found.

**Why it matters:** This maps the mail and hosting setup. The SPF record ends in `~all` (soft fail), and the DNS software version string is exposed.

## 4. Consolidated Target Profile

| Area | Finding |
|---|---|
| Registrar | GoDaddy |
| Hosting / DNS | HostGator, cPanel |
| IP address | 192.232.216.135 |
| Web server | Apache |
| CMS | WordPress 7.1.4 with WP Download Manager 3.3.76 |
| WAF | ModSecurity (SpiderLabs) |
| Mail | mail.networkwalks.com, SPF soft fail |
| DNSSEC | Not enabled |

## 5. Risk Analysis

These are observations from information gathering, not confirmed vulnerabilities. No exploitation or vulnerability validation was performed.

| # | Finding | Evidence | Potential impact | Level |
|---|---|---|---|---|
| 1 | Web technology information exposed | WhatWeb identified Apache, WordPress 7.1.4 and WP Download Manager 3.3.76 | Attackers may use version information to find software needing review | Medium |
| 2 | Server IP address identifiable | Nslookup resolved the domain to 192.232.216.135 | Reveals the network location of the web service | Low |
| 3 | HTTP technical information exposed | Curl showed the Apache banner, WordPress caching headers and redirect plugin | May assist fingerprinting and further enumeration | Low |
| 4 | WAF technology identifiable | Wafw00f identified ModSecurity (SpiderLabs) | Reveals part of the security architecture | Low |
| 5 | DNS software version disclosed | DNSRecon showed BIND 9.16.23-RH plus mail and cPanel service records | Helps build a broader infrastructure profile | Medium |
| 6 | Email and DNS hardening gaps | SPF ends in `~all` (soft fail) and DNSSEC is unsigned | Weaker protection against email spoofing and DNS tampering | Low |
| 7 | Public contact email exposed | WhatWeb exposed `info@networkwalks.com` | Can be harvested for phishing or social engineering | Low |

## 6. Recommendations

1. **Reduce version disclosure:** remove the WordPress generator tag, minimize web server banners and configure DNS software not to reveal its version.
2. **Keep software updated:** patch WordPress core and plugins such as WP Download Manager, and remove unused plugins.
3. **Review HTTP headers and cookies** for unnecessary technical information.
4. **Strengthen DNS and email authentication:** enable DNSSEC where supported, and after validating legitimate senders move SPF from `~all` to `-all` and confirm DKIM and DMARC.
5. **Keep the WAF enabled, tuned and monitored.**
6. **Run the same recon tools against your own assets periodically** to see what an attacker would see.

## 7. Lessons Learned

- Public information alone builds a surprisingly complete target profile.
- Each tool shows one slice; the value comes from combining them.
- None of these tools attack the target, which is why recon is powerful and hard to detect.
- Local DNS resolver issues can cause misleading timeouts, so verify and retry before drawing conclusions.
- Offensive recon knowledge tells defenders what to hide and what to monitor.

## 8. Conclusion

Footprinting showed how much can be learned about a website using only public information and a handful of built-in Kali tools. These findings form the foundation for the scanning phase and later testing stages, because a target cannot be assessed until it is understood. All activity was completed within an authorized educational lab scope.

## Disclaimer

These materials are for education and research only. Do not use them against systems you do not own or lack explicit written authorization to test.
