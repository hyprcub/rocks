---
layout: post
author: hyprcub
title: Web Enumeration
tags: cheatsheet
---
# Basic Enumeration

| Command                                                           | Description                                |
|-------------------------------------------------------------------|--------------------------------------------|
| `curl -is http://$target/robots.txt`                              | Check the headers and `robots.txt` content |
| `curl -iks https://$target/robots.txt`                            | Same if SSL enabled                        |
| `curl -is http://$target` or *View Page Source* from your browser | Read the source code **carefully!**        |
| `whatweb --no-errors -a 3 -v http://$target`                      | Recognises web technologies                |

# Url Bruteforcing

## Wordlists

| List                                                                                 | # of lines | Description             |
|--------------------------------------------------------------------------------------|------------|-------------------------|
| `export dict=/usr/share/seclists/Discovery/Web-Content/common.txt`                   | ~5k        | Directory/Page Wordlist |
| `export dict=/usr/share/dirb/wordlists/big.txt`                                      | ~20k       | Directory/Page Wordlist |
| `export dict=/usr/share/dirbuster/wordlists/directory-list-lowercase-2.3-medium.txt` | ~200k      | Directory/Page Wordlist |

## Commands

| Command                                                                     | Description                                 |
|-----------------------------------------------------------------------------|---------------------------------------------|
| `ffuf -w $dict -u http://$target/FUZZ -c -t 50`                             | Colored output, 50 threads                  |
| `ffuf -w $dict -u http://$target/FUZZ -c -t 50 -o url_bruteforce.md -of md` | Saves results to a Markdown file            |
| `ffuf -w $dict -u http://$target/FUZZ -c -t 50 -x .txt,.php`                | Tests also for extensions `.txt` and `.php` |
 
# Website Crawling

## References

- Credits to [Stök](https://twitter.com/stokfredrik)
- [Hakrawler](https://github.com/hakluke/hakrawler)
- [httpx](https://github.com/projectdiscovery/httpx)

## Installation 

```bash
go get github.com/hakluke/hakrawler && GO111MODULE=on go get -v github.com/projectdiscovery/httpx/cmd/httpx && export PATH=$PATH:$HOME/go/bin
```

## Usage

1. [Burp suite](https://portswigger.net/burp) with **intercept off**
2. `hakrawler -url http://$target -depth 3 -plain | httpx --http-proxy http://127.0.0.1:8080`

# Vulnerability Scanning

| Command                                                                              | Description                                                                        |
|--------------------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `nikto -Display 1234EP -o nikto.txt -Format txt -Tuning 123bde -host $target -C all` | Scan for various default and insecure files, configurations and programs           |
| `ls /usr/share/nmap/scripts/*http*`                                                  | List various [Nmap](https://nmap.org/) scripts                                     |
| `nmap $target -p 80 -sV --script http-shellshock`                                    | Execute some basic enumeration scripts and look for shellshock vulnerability       |
| `wpscan --url http://$target`                                                        | Scan a WordPress installation using [wpscan](https://github.com/wpscanteam/wpscan) |

Look also for any *you_name_it_CMS_scanner*, like:
- [Droopescan](https://github.com/droope/droopescan) which can scan Drupal installation
- [OWASP JoomScan](https://github.com/OWASP/joomscan) which can scan JoomScan CMS.
