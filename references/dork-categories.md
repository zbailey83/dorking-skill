# Extended Google Dork Library

> 100+ ready-to-use dorks organized by attack surface and use case.
> Substitute `example.com` with your target domain where applicable.

---

## Table of Contents

1. [Web Application Discovery](#1-web-application-discovery)
2. [File & Document Exposure](#2-file--document-exposure)
3. [Database & Backend Exposure](#3-database--backend-exposure)
4. [Credential & Secret Leaks](#4-credential--secret-leaks)
5. [Infrastructure & Network Recon](#5-infrastructure--network-recon)
6. [CMS & Platform Fingerprinting](#6-cms--platform-fingerprinting)
7. [IoT & Camera Discovery](#7-iot--camera-discovery)
8. [Cloud Storage & S3 Buckets](#8-cloud-storage--s3-buckets)
9. [Code Repositories & Paste Sites](#9-code-repositories--paste-sites)
10. [People & OSINT](#10-people--osint)
11. [Competitive Intelligence](#11-competitive-intelligence)
12. [Error & Debug Pages](#12-error--debug-pages)
13. [Email & Communication](#13-email--communication)
14. [Legal & Compliance](#14-legal--compliance)

---

## 1. Web Application Discovery

```
# Generic admin panels
inurl:/admin intitle:"Login"
inurl:/admin/login
inurl:admin/index.php
inurl:administrator/index.php
inurl:/wp-admin/
inurl:/wp-login.php
inurl:/user/login
inurl:/auth/login
inurl:/signin
inurl:/account/login

# API endpoints
inurl:/api/v1/ site:example.com
inurl:/api/v2/ site:example.com
inurl:swagger site:example.com
inurl:api-docs site:example.com
intitle:"Swagger UI" site:example.com

# Development & staging environments
inurl:dev.example.com
inurl:staging.example.com
inurl:test.example.com
inurl:beta.example.com
inurl:sandbox.example.com
inurl:uat.example.com
site:*.example.com -www -mail

# Internal tools
inurl:jenkins site:example.com
inurl:jira site:example.com
inurl:confluence site:example.com
inurl:gitlab site:example.com
inurl:bitbucket site:example.com
inurl:sonarqube site:example.com
inurl:grafana site:example.com
```

---

## 2. File & Document Exposure

```
# Office documents
site:example.com filetype:pdf
site:example.com filetype:docx OR filetype:doc
site:example.com filetype:xlsx OR filetype:xls OR filetype:csv
site:example.com filetype:pptx OR filetype:ppt

# Sensitive document indicators
filetype:pdf intext:"internal use only" site:example.com
filetype:pdf intext:"confidential" site:example.com
filetype:pdf intext:"do not distribute" site:example.com
filetype:docx intext:"draft" site:example.com

# Configuration files
filetype:env intext:"password"
filetype:cfg intext:"password"
filetype:ini intext:"password"
filetype:conf intext:"password"
filetype:yml intext:"password"
filetype:yaml intext:"password"
filetype:json intext:"password"
filetype:xml intext:"password"

# Backup files
filetype:bak site:example.com
filetype:old site:example.com
filetype:backup site:example.com
filetype:sql site:example.com
filetype:dump site:example.com
inurl:wp-config.bak
inurl:database.bak

# Log files
filetype:log intext:"password"
filetype:log intext:"error" site:example.com
filetype:log inurl:access
filetype:log inurl:error

# Private keys & certificates
filetype:pem intext:"BEGIN CERTIFICATE"
filetype:key intext:"BEGIN RSA PRIVATE KEY"
filetype:ppk intext:"PuTTY-User-Key-File"
```

---

## 3. Database & Backend Exposure

```
# phpMyAdmin
intitle:"phpMyAdmin" inurl:phpmyadmin
inurl:phpmyadmin/index.php
intitle:"Welcome to phpMyAdmin"

# Database dumps
filetype:sql intext:"INSERT INTO"
filetype:sql intext:"CREATE TABLE"
filetype:sql "phpMyAdmin SQL Dump"
filetype:sql intext:"Dumping data for table"

# MongoDB
intitle:"MongoDB" inurl:28017
intitle:"MongoBooster" inurl:3000

# Elasticsearch / Kibana
intitle:"Kibana" inurl:5601
intitle:"Elastic" inurl:9200
inurl:9200/_cat/indices
intitle:"Index Status" inurl:9200

# Redis
intitle:"Redis" inurl:6379

# Exposed database connection strings
intext:"Data Source=" filetype:config
intext:"connectionString" filetype:web.config
intext:"mysql_connect" filetype:php
intext:"pg_connect" filetype:php
```

---

## 4. Credential & Secret Leaks

```
# Generic credential exposure
intext:"password=" filetype:log
intext:"password:" filetype:yaml
intext:"passwd=" filetype:conf
intext:"db_password" filetype:env

# API keys
intext:"api_key" filetype:json
intext:"api_secret" filetype:json
intext:"access_token" filetype:json
intext:"client_secret" filetype:json
intext:"client_id" filetype:json

# AWS credentials
intext:"aws_access_key_id" filetype:csv
intext:"aws_secret_access_key" filetype:txt
intext:"AKIA" filetype:txt
site:pastebin.com "aws_secret_access_key"

# SSH / Private keys
filetype:pem "PRIVATE KEY"
filetype:id_rsa
filetype:ppk
"BEGIN OPENSSH PRIVATE KEY" filetype:txt
"BEGIN RSA PRIVATE KEY" site:pastebin.com

# GitHub tokens
intext:"ghp_" filetype:txt
intext:"github_token" filetype:env

# Slack tokens
intext:"xoxb-" OR intext:"xoxa-" filetype:txt

# Google service accounts
filetype:json "type" "service_account"

# .htpasswd files
inurl:.htpasswd
filetype:htpasswd
```

---

## 5. Infrastructure & Network Recon

```
# VPN & remote access
inurl:vpn.example.com
inurl:remote.example.com
intitle:"SSL VPN" inurl:login
intitle:"Pulse Secure" inurl:login
intitle:"Cisco AnyConnect" inurl:login
intitle:"Citrix" inurl:login

# Network monitoring
intitle:"Nagios" inurl:nagios
intitle:"Zabbix" inurl:zabbix
intitle:"PRTG Network Monitor" inurl:PRTG
intitle:"Cacti" inurl:graph_view.php
intitle:"Netdata" inurl:netdata

# Router & firewall panels
intitle:"pfSense" inurl:index.php
intitle:"DD-WRT" inurl:userinfo.htm
intitle:"OpenWRT" inurl:luci

# Server status pages
intitle:"Apache Status" inurl:server-status
intitle:"nginx status" inurl:nginx_status
intitle:"IIS Windows Server" inurl:iis.htm

# SNMP exposure
inurl:snmpd.conf filetype:conf

# Open ports / service banners
intitle:"SSH-2.0" inurl:index
```

---

## 6. CMS & Platform Fingerprinting

```
# WordPress
site:example.com inurl:wp-content
site:example.com inurl:wp-includes
site:example.com filetype:php inurl:wp-
inurl:wp-content/plugins filetype:txt "readme.txt"
inurl:wp-content/themes filetype:txt "readme.txt"
inurl:wp-login.php "powered by WordPress"

# Joomla
site:example.com inurl:components/com_
inurl:/administrator site:example.com joomla
intitle:"Joomla!" inurl:administrator

# Drupal
site:example.com inurl:sites/default/files
inurl:CHANGELOG.txt "Drupal"
intitle:"Drupal" inurl:user/login

# Magento
site:example.com inurl:/admin "Magento"
inurl:downloader/index.php "Magento"

# Shopify
site:example.myshopify.com

# Django / Python
inurl:debug=True site:example.com
intitle:"OperationalError" site:example.com
intitle:"Django" "Error" site:example.com

# Laravel
inurl:storage/logs site:example.com
inurl:artisan site:example.com
```

---

## 7. IoT & Camera Discovery

```
# Generic camera dorks
inurl:/view/index.shtml
inurl:ViewerFrame?Mode=
inurl:axis-cgi/mjpg
inurl:MultiCameraFrame?Mode=
inurl:/mjpg/video.mjpg

# Axis cameras
intitle:"Live View / - AXIS"
inurl:/axis-cgi/jpg/image.cgi
intitle:"AXIS" inurl:bitmap

# Sony cameras
intitle:"snc-rz30" inurl:home/
intitle:"SNC-Z20" inurl:home/

# Panasonic cameras
intitle:"Panasonic Network Camera" inurl:top.htm

# D-Link cameras
inurl:view/viewer_index.shtml "D-Link"

# GeoVision cameras
intitle:"GeoVision" inurl:serverpage
inurl:GeoCam?Cam=

# Networked printers
intitle:"HP LaserJet" inurl:info_configuration.htm
intitle:"Xerox" inurl:status

# Industrial / SCADA
intitle:"SCADA" inurl:login
intitle:"Modbus" inurl:index
intitle:"HMI" inurl:login
```

---

## 8. Cloud Storage & S3 Buckets

```
# Amazon S3
site:s3.amazonaws.com intitle:"index of"
site:s3.amazonaws.com "bucket"
inurl:.s3.amazonaws.com
site:s3-us-west-2.amazonaws.com
inurl:s3.amazonaws.com/company-name

# Azure Blob
site:blob.core.windows.net
inurl:.blob.core.windows.net

# Google Cloud Storage
site:storage.googleapis.com
inurl:storage.googleapis.com/bucket

# Digital Ocean Spaces
site:digitaloceanspaces.com intitle:"index of"

# Generic open buckets
intitle:"index of" "AmazonS3"
intitle:"index of" cloud-bucket
```

---

## 9. Code Repositories & Paste Sites

```
# Pastebin
site:pastebin.com "example.com" password
site:pastebin.com "@example.com"
site:pastebin.com "BEGIN RSA PRIVATE KEY"
site:pastebin.com "aws_access_key_id"
site:pastebin.com "api_key"

# Other paste sites
site:justpaste.it "example.com"
site:hastebin.com "example.com"
site:gist.github.com "example.com" password
site:ghostbin.com "example.com"

# GitHub (note: GitHub has its own search, but Google still indexes public repos)
site:github.com "example.com" password
site:github.com "example.com" "api_key"
site:github.com "example.com" filetype:env
site:github.com intext:"DB_PASSWORD" filetype:env

# GitLab
site:gitlab.com "example.com" "password"
```

---

## 10. People & OSINT

```
# LinkedIn
site:linkedin.com/in "works at" "company name"
site:linkedin.com "company name" "CEO" OR "CTO" OR "CISO"
site:linkedin.com/pub "company name"

# Social media
site:twitter.com "example.com"
site:facebook.com "example.com"

# Email discovery
"@example.com" filetype:xlsx
"@example.com" filetype:csv
"@example.com" filetype:pdf
site:example.com intext:"@example.com"

# Phone numbers
site:example.com intext:"tel:" OR intext:"phone:"
"company name" filetype:pdf intext:"phone"

# Physical addresses
site:example.com intext:"headquarters" OR intext:"office"
"company name" intext:"address" filetype:pdf

# Executive info
"company name" "CEO" OR "President" email "@"
"company name" "annual report" filetype:pdf
"company name" "board of directors" filetype:pdf
```

---

## 11. Competitive Intelligence

```
# Job postings reveal tech stack
site:jobs.lever.co "company name"
site:greenhouse.io "company name"
site:indeed.com "company name" "engineer"
"company name" inurl:jobs OR inurl:careers site:example.com

# Tech stack from job descriptions
"company name" jobs intext:"we use" OR intext:"our stack"
site:linkedin.com/jobs "company name"

# Press releases & announcements
site:prnewswire.com "company name"
site:businesswire.com "company name"

# Financial info
site:sec.gov "company name" 10-K
"company name" filetype:pdf "annual report"
"company name" "quarterly results" filetype:pdf

# Contract & vendor info
site:usaspending.gov "company name"
"company name" "request for proposal" OR "RFP" filetype:pdf
```

---

## 12. Error & Debug Pages

```
# PHP errors
intext:"Warning: mysql_connect()" site:example.com
intext:"Fatal error:" site:example.com
intext:"Parse error:" site:example.com
intext:"Notice: Undefined" site:example.com

# ASP.NET errors
intext:"Server Error in Application" site:example.com
intext:"Stack Trace:" intext:"Server Error" site:example.com
inurl:elmah.axd intext:"Error Log"
intext:"Object reference not set" site:example.com

# Django / Python
intitle:"OperationalError at"
intitle:"ProgrammingError at"
intitle:"DisallowedHost at"

# Java errors
intext:"java.lang.NullPointerException" site:example.com
intext:"java.sql.SQLException" site:example.com
intitle:"HTTP Status 500" site:example.com

# Generic debug info
intext:"phpinfo()" intitle:"phpinfo()"
inurl:phpinfo.php
intitle:"Test Page for Apache" "It Works"
```

---

## 13. Email & Communication

```
# Email servers
intitle:"Roundcube Webmail" inurl:webmail
intitle:"SquirrelMail" inurl:webmail
intitle:"Outlook Web App" inurl:owa
intitle:"Zimbra Web Client" inurl:zimbra
intitle:"Horde" inurl:imp

# Mailing lists & archives
site:groups.google.com "example.com"
site:listserv.example.com
inurl:mailman site:example.com

# Newsletter & marketing
site:mailchimp.com "example.com"
site:constantcontact.com "example.com"
```

---

## 14. Legal & Compliance

```
# Court records & legal filings
site:pacer.gov "company name"
site:courtlistener.com "company name"
"company name" filetype:pdf "settlement"
"company name" filetype:pdf "lawsuit"

# Regulatory filings
site:sec.gov "company name"
site:ftc.gov "company name"
"company name" site:regulations.gov

# GDPR & privacy
site:example.com inurl:privacy-policy
site:example.com inurl:gdpr
site:example.com filetype:pdf "data processing agreement"

# Trademark & IP
site:tmsearch.uspto.gov "company name"
"company name" filetype:pdf "patent"
```

---

*This reference file supplements the main SKILL.md. Return to it when you need a specific dork category not covered in the quick reference.*
