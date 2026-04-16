# 🔍 Google Dorking Skill

> **Skill path:** `/mnt/skills/user/google-dorking/SKILL.md`

Advanced search operator framework for OSINT, targeted data retrieval, vulnerability discovery, and agent-driven recon workflows.

> ⚖️ **Ethical Use Only** — Google Dorking is a legal and legitimate research technique. Always operate within legal boundaries, respect robots.txt and terms of service, and only target systems you own or have explicit permission to test. Never use these techniques for unauthorized access.

---

## When This Skill Triggers

| Keyword / Request | Example |
|---|---|
| Dork / dorking / google hacking | "write me a dork for exposed login panels" |
| OSINT / recon / attack surface mapping | "help me do OSINT on this company" |
| Search operators: `site:` `inurl:` `filetype:` `intitle:` `intext:` | "find all PDFs on example.com" |
| Exposure detection / data leak discovery | "find exposed env files on this domain" |
| Competitive intelligence via search | "find what files a competitor has indexed" |
| Automated / agent search workflows | "build a dorking loop for this target list" |

---

## Core Operators — Quick Reference

| Operator | Syntax | What It Does |
|---|---|---|
| `site:` | `site:example.com` | Restrict results to a specific domain |
| `inurl:` | `inurl:admin` | Match text in the URL |
| `intitle:` | `intitle:"index of"` | Match text in the page title |
| `intext:` | `intext:"password"` | Match text in the page body |
| `filetype:` | `filetype:pdf` | Filter by file extension |
| `ext:` | `ext:sql` | Alias for `filetype:` |
| `before:` / `after:` | `after:2023-01-01` | Filter by publication date |
| `"exact phrase"` | `"open directory"` | Force an exact multi-word match |
| `-word` | `-inurl:https` | Exclude a word or operator result |
| `OR` / `\|` | `filetype:pdf OR filetype:docx` | Boolean OR |
| `*` | `"admin * login"` | Wildcard match |
| `site:*.domain.com` | `site:*.example.com -www` | Enumerate all subdomains |

---

## OSINT Use Cases & Dork Templates

### 1. Open Directory Listings

```
intitle:"index of" site:example.com
intitle:"index of /" "parent directory"
intitle:"index of" (mp4 OR avi OR mkv)
intitle:"index of" "backup" OR "bak" OR "old"
```

### 2. Exposed Documents & Files

```
site:example.com filetype:pdf
site:example.com filetype:xlsx OR filetype:csv
filetype:sql intext:"INSERT INTO"
filetype:log intext:"password"
filetype:env "DB_PASSWORD" OR "SECRET_KEY"
filetype:pem OR filetype:key "BEGIN RSA PRIVATE KEY"
```

### 3. Login Panels & Admin Interfaces

```
inurl:/admin/login site:example.com
inurl:"/wp-admin" site:example.com
intitle:"Admin Panel" inurl:admin
inurl:/phpmyadmin
inurl:/cpanel intext:"login"
```

### 4. Exposed Cameras & IoT

```
inurl:/view/index.shtml
intitle:"Live View / - AXIS"
inurl:axis-cgi/mjpg
inurl:ViewerFrame?Mode=
```

### 5. Subdomain & Infrastructure Discovery

```
site:*.example.com -www
site:*.example.com inurl:api
inurl:dev.example.com OR inurl:staging.example.com
inurl:jenkins site:example.com
inurl:gitlab site:example.com
```

### 6. Credentials & Secrets

```
intext:"api_key" filetype:json
intext:"aws_access_key_id" filetype:txt
filetype:cfg intext:"password"
filetype:ini intext:"password="
inurl:config intext:"password"
```

### 7. Company & Employee Intelligence

```
site:linkedin.com/in "works at" "company name"
"@companyname.com" filetype:xlsx OR filetype:csv
site:example.com intext:"@example.com"
site:example.com inurl:team OR inurl:staff
```

### 8. Paste Sites & Data Leaks

```
site:pastebin.com "example.com"
site:pastebin.com "password" "@example.com"
site:pastebin.com intext:"BEGIN RSA PRIVATE KEY"
site:pastebin.com intext:"aws_secret_access_key"
```

---

## Power Combinations

```bash
# Exposed backup files on a specific domain
site:example.com filetype:bak OR filetype:old OR filetype:backup

# Admin panels on government sites
site:gov intitle:"admin" inurl:login

# Exposed environment configs (exclude GitHub)
filetype:env OR filetype:cfg intext:"password" -site:github.com

# Open Elasticsearch / Kibana instances
intitle:"Kibana" inurl:5601
intitle:"Elastic" inurl:9200

# Exposed AWS S3 buckets
site:s3.amazonaws.com intitle:"index of"
```

---

## Agent Methodology — 4 Phases

### Phase 1 — Scope Definition
- Identify target: domain, org name, IP range, personnel
- Define objective: leaked data? subdomains? credentials? login panels?
- Select relevant dork categories
- Set time scope with `before:` / `after:` if relevant

### Phase 2 — Query Construction
1. Start broad: `site:example.com`
2. Layer operators: `site:example.com filetype:pdf`
3. Add content filter: `site:example.com filetype:pdf intext:"confidential"`
4. Refine with negation: add `-inurl:privacy` etc.

### Phase 3 — Results Analysis
- Capture result URLs, titles, and snippets
- Categorize findings by type (document, panel, credential, etc.)
- Flag high-priority results for deeper investigation
- Cross-reference with other OSINT sources

### Phase 4 — Reporting
- List every dork query used
- Document each finding: URL · dork query · risk level · description
- Prioritize by severity: **Critical → High → Medium → Low → Informational**
- Recommend remediation (de-index files, restrict panels, rotate credentials)

---

## Agent Automation — Example Loop

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

**Automation tips:**
- Pace requests — Google rate-limits aggressive queries
- Use Google Custom Search API or SerpAPI over scraping
- Rotate query order to avoid duplicate filtering
- De-duplicate results by URL across multiple dorks
- Screenshot findings at discovery time for security reports
- Chain with: `wget`, `curl`, `nmap`, `whois`, `theHarvester`

---

## Common Mistakes to Avoid

| ❌ Mistake | ✅ Fix |
|---|---|
| Using `filetype:` without a domain scope | Add `site:` to reduce false positives |
| Stacking too many operators at once | Build up incrementally; 2–3 operators max per query |
| Forgetting to exclude noise domains | Use `-site:wikipedia.org -site:stackoverflow.com` |
| Ignoring pagination | Results often extend beyond page 1 |
| Not quoting multi-word phrases | Always quote: `"open directory"` not `open directory` |
| Overlooking date filters | Add `after:` to get recent/relevant results |

---

## Quick Cheatsheet by Objective

```bash
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

# What tech are they running?
site:example.com intitle:"powered by" OR intext:"powered by"
```

---

## External Resources

| Resource | URL | Purpose |
|---|---|---|
| Exploit-DB GHDB | `exploit-db.com/google-hacking-database` | Categorized dork library |
| OSINT Framework | `osintframework.com` | Comprehensive OSINT tooling map |
| Shodan | `shodan.io` | Internet-wide infrastructure & IoT recon |
| Censys | `censys.io` | Certificate and internet scan data |

---

*Trigger keywords: dork · OSINT · inurl: · site: · filetype: · recon · google hacking · attack surface · data leak · subdomain enumeration*
