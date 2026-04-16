---
name: google-dorking
description: >
  Use this skill for Google Dorking, advanced search operator usage, OSINT (Open Source Intelligence),
  targeted data retrieval, recon, and agent-driven search workflows. Trigger whenever the user wants to:
  find exposed files, leaked data, login panels, subdomains, indexed documents, camera feeds, vulnerable
  endpoints, email addresses, phone numbers, employee info, or any search-based intelligence gathering.
  Also trigger for requests involving "dork", "dorks", "google hacking", "search operators", "site:",
  "inurl:", "filetype:", "intitle:", "intext:", exposure detection, OSINT recon, attack surface mapping,
  competitive intelligence via search, content discovery, or automated search queries. Use aggressively —
  if the user is trying to find something specific on the web using targeted search techniques, this skill applies.
---

# Google Dorking Skill

A comprehensive reference for crafting, combining, and automating advanced Google search operators
for OSINT, recon, data retrieval, vulnerability discovery, and agent search workflows.

> ⚖️ **Ethical Use**: Google Dorking is a legal and legitimate research technique. Always operate
> within legal boundaries, respect robots.txt and terms of service, and only target systems you
> own or have explicit permission to test. Never use these techniques for unauthorized access.

---

## Quick Reference: Core Operators

| Operator | Syntax | What it does |
|---|---|---|
| `site:` | `site:example.com` | Restrict results to a specific domain |
| `inurl:` | `inurl:admin` | Match text in the URL |
| `intitle:` | `intitle:"index of"` | Match text in the page title |
| `intext:` | `intext:"password"` | Match text in the page body |
| `filetype:` | `filetype:pdf` | Filter by file extension |
| `ext:` | `ext:sql` | Alias for filetype: |
| `cache:` | `cache:example.com` | View Google's cached version |
| `link:` | `link:example.com` | Pages linking to a URL (deprecated but still functional) |
| `related:` | `related:example.com` | Similar sites |
| `info:` | `info:example.com` | Info about a domain |
| `define:` | `define:ransomware` | Define a term |
| `numrange:` | `numrange:1000-5000` | Match numbers in a range |
| `before:` | `before:2023-01-01` | Results published before date |
| `after:` | `after:2022-01-01` | Results published after date |
| `"exact phrase"` | `"open directory"` | Exact phrase match |
| `-word` | `-inurl:https` | Exclude a word or operator |
| `*` | `"admin * login"` | Wildcard |
| `OR` / `\|` | `filetype:pdf OR filetype:docx` | Boolean OR |

---

## OSINT Use Cases & Dork Templates

### 🔍 1. Open Directory Listings

Find exposed file directories:
```
intitle:"index of" site:example.com
intitle:"index of /" "parent directory"
intitle:"index of" (mp4 OR avi OR mkv)
intitle:"index of" "backup" OR "bak" OR "old"
intitle:"index of" inurl:ftp
```

### 📄 2. Exposed Documents & Files

Find sensitive documents indexed by Google:
```
site:example.com filetype:pdf
site:example.com filetype:xlsx OR filetype:csv
site:gov filetype:pdf "confidential"
filetype:doc "internal use only"
filetype:xls inurl:"email" OR inurl:"contact"
filetype:sql intext:"INSERT INTO"
filetype:log intext:"password"
filetype:env "DB_PASSWORD" OR "SECRET_KEY"
filetype:pem OR filetype:key "BEGIN RSA PRIVATE KEY"
filetype:bak inurl:wp-config
```

### 🔐 3. Login Panels & Admin Interfaces

Locate exposed admin or login pages:
```
inurl:/admin/login site:example.com
inurl:"/wp-admin" site:example.com
intitle:"Admin Panel" inurl:admin
inurl:login intitle:"admin"
inurl:/phpmyadmin
inurl:/cpanel intext:"login"
intitle:"Plesk" inurl:login
intitle:"Webmin" inurl:10000
inurl:/admin intext:"username" "password"
```

### 📷 4. Exposed Cameras & IoT Devices

Find public-facing cameras and devices:
```
inurl:/view/index.shtml
intitle:"Live View / - AXIS"
inurl:axis-cgi/mjpg
intitle:"Network Camera" inurl:axis
intitle:"webcam" inurl:view
inurl:ViewerFrame?Mode=
intitle:"GeoVision" inurl:serverpage
inurl:/mjpg/video.mjpg
intitle:"IP Camera" inurl:LvAppl
```

### 🏢 5. Company & Employee Intelligence

Gather organizational intel:
```
site:linkedin.com/in "works at" "company name"
site:linkedin.com "company name" "email"
"@companyname.com" filetype:xlsx OR filetype:csv
site:example.com intext:"@example.com"
"company name" filetype:pdf "employee directory"
site:example.com inurl:team OR inurl:staff OR inurl:about
"company name" site:glassdoor.com
```

### 🌐 6. Subdomain & Infrastructure Discovery

Enumerate subdomains and infrastructure:
```
site:*.example.com -www
site:*.example.com -www -mail
site:*.example.com inurl:api
site:*.example.com intitle:"dashboard"
inurl:vpn.example.com OR inurl:remote.example.com
inurl:dev.example.com OR inurl:staging.example.com
inurl:jenkins site:example.com
inurl:jira site:example.com
inurl:gitlab site:example.com
```

### 🔑 7. Credentials & Secrets

Find accidentally exposed credentials:
```
intext:"api_key" filetype:json
intext:"access_token" filetype:json
intext:"aws_access_key_id" filetype:txt
"mongodb://user:password@" site:pastebin.com
filetype:cfg intext:"password"
filetype:ini intext:"password="
site:github.com "password" filetype:json
inurl:config intext:"password"
```

### 💻 8. Vulnerable Endpoints & Tech Fingerprinting

Identify potentially vulnerable software versions:
```
inurl:struts2 filetype:action
intitle:"Powered by Apache" inurl:server-status
intitle:"phpinfo()" inurl:phpinfo.php
inurl:joomla intext:"powered by"
intext:"SQL syntax" OR intext:"mysql_fetch_array"
intitle:"Test Page for Apache" inurl:apache2
inurl:wp-content/plugins filetype:php "Changelog"
intitle:"Drupal" inurl:CHANGELOG.txt
inurl:elmah.axd intext:"Error Log"
```

### 📧 9. Email & Contact Harvesting

Collect email addresses and contacts:
```
site:example.com intext:"@example.com"
"email" filetype:csv site:example.com
"contact" OR "email" filetype:xlsx site:example.com
"@gmail.com" "password" site:pastebin.com
intext:"email" site:linkedin.com "company name"
```

### 📰 10. Paste Sites & Data Leaks

Search paste sites for leaked data:
```
site:pastebin.com "example.com"
site:pastebin.com "password" "@example.com"
site:pastebin.com intext:"BEGIN RSA PRIVATE KEY"
site:pastebin.com intext:"aws_secret_access_key"
site:justpaste.it "example.com"
site:hastebin.com "example.com"
```

---

## Operator Combination Patterns

### Power Combining

Chain operators for precision targeting:
```
# Exposed backup files on a specific domain
site:example.com filetype:bak OR filetype:old OR filetype:backup

# Admin panels on government sites
site:gov intitle:"admin" inurl:login

# PDF documents containing email addresses on a corporate domain
site:example.com filetype:pdf intext:"@example.com"

# Publicly exposed environment configs
filetype:env OR filetype:cfg intext:"password" -site:github.com

# Open Elasticsearch instances
intitle:"Kibana" inurl:5601
intitle:"Elastic" inurl:9200

# Exposed AWS S3 bucket indexes
site:s3.amazonaws.com intitle:"index of"
site:.s3.amazonaws.com "bucket"
```

### Negation Patterns

Use `-` to refine and eliminate noise:
```
# Remove irrelevant domains
site:*.example.com -www -mail -smtp

# Exclude common "how to" articles from vuln searches
inurl:wp-admin -site:wordpress.com -intitle:"how to"

# Exclude HTTPS (find legacy HTTP admin panels)
inurl:admin -inurl:https

# Exclude news results
intext:"internal use only" -site:news.google.com
```

---

## Agent Workflow: Systematic Dorking

When executing a dorking campaign systematically (e.g., for recon or security assessment):

### Phase 1: Scope Definition
```
1. Identify target: domain, org name, IP range, personnel
2. Define objective: leaked data? subdomains? credentials? login panels?
3. Select relevant dork categories from above
4. Set time scope with before:/after: if temporal relevance matters
```

### Phase 2: Query Construction
```
1. Start broad: site:example.com
2. Layer operators: site:example.com filetype:pdf
3. Add content filter: site:example.com filetype:pdf intext:"confidential"
4. Refine with negation: site:example.com filetype:pdf intext:"confidential" -inurl:privacy
```

### Phase 3: Results Analysis
```
1. Capture result URLs, titles, and snippets
2. Categorize findings by type (document, panel, credential, etc.)
3. Flag high-priority results for deeper investigation
4. Document dork queries used and result counts
5. Cross-reference with other OSINT sources
```

### Phase 4: Reporting
```
- List dorks used
- Document each finding with: URL, dork query, risk level, description
- Prioritize by severity: Critical > High > Medium > Low > Informational
- Recommend remediation (de-index sensitive files, restrict admin panels, rotate creds)
```

---

## Google Dork Databases & Resources

For pre-built dork collections, reference:
- **Exploit-DB Google Hacking Database (GHDB)**: `https://www.exploit-db.com/google-hacking-database`
- **Pentest-Tools Dork List**: Organized by category
- **OSINT Framework**: `https://osintframework.com`
- **Shodan.io**: Complements Google dorking for infrastructure/IoT recon (not Google-based)
- **Censys.io**: Certificate and internet-wide scan data

---

## Automation Tips for Agents

When automating Google Dorking as an agent:

1. **Pace requests** — Google rate-limits aggressive queries. Space them with delays.
2. **Use search APIs** — Prefer Google Custom Search API or SerpAPI over scraping.
3. **Rotate queries** — Vary operator order to avoid duplicate filtering.
4. **Capture metadata** — Log query, timestamp, result count, top URLs, and snippets.
5. **De-duplicate** — Same pages appear for multiple dorks; deduplicate by URL.
6. **Screenshot evidence** — For security reports, capture page state at discovery time.
7. **Chain with other tools** — Feed URLs to tools like `wget`, `curl`, `nmap`, `whois`, `theHarvester`.

### Example Agent Query Loop (pseudocode)
```python
dorks = [
    'site:{target} filetype:pdf intext:"confidential"',
    'site:{target} inurl:admin intitle:"login"',
    'site:*.{target} -www',
    'site:{target} filetype:env OR filetype:cfg',
]

findings = []
for dork in dorks:
    query = dork.format(target="example.com")
    results = search_api.query(query, num=10)
    for r in results:
        findings.append({
            "dork": query,
            "url": r.url,
            "title": r.title,
            "snippet": r.snippet
        })
    time.sleep(2)  # Rate limit buffer

report(findings)
```

---

## Common Mistakes to Avoid

| ❌ Mistake | ✅ Fix |
|---|---|
| Using `filetype:` without a domain scope | Add `site:` to reduce false positives |
| Stacking too many operators at once | Build up incrementally; 2–3 operators max per query |
| Forgetting to exclude common noise domains | Use `-site:wikipedia.org -site:stackoverflow.com` etc. |
| Ignoring pagination | GHDB dorks often have results beyond page 1 |
| Using cache: on HTTPS pages | Cache may not work on secured pages |
| Not quoting multi-word phrases | Always quote: `"open directory"` not `open directory` |
| Overlooking date filters | Add `after:` to get recent/relevant results |

---

## Quick Dork Cheatsheet by Objective

```
# What's on this domain?
site:example.com

# What files are exposed?
site:example.com filetype:pdf OR filetype:xls OR filetype:doc OR filetype:csv

# Any admin panels?
site:example.com inurl:admin OR inurl:login OR inurl:dashboard

# Any subdomains?
site:*.example.com -www

# Any secrets or configs?
site:example.com filetype:env OR filetype:cfg OR filetype:ini OR filetype:bak

# Any employee info?
site:linkedin.com/in "example company"
"@example.com" filetype:xlsx

# What tech are they running?
site:example.com intitle:"powered by" OR intext:"powered by"
```

---

Read `references/dork-categories.md` for an extended categorized dork library with 100+ ready-to-use dorks organized by attack surface.
