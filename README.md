# One Liner Command For Bug Hunting That No-One Tell You!
---

### **1️⃣ Subdomain Enumeration & Resolving**  
```bash
subfinder -d target.com | anew subdomains.txt | httprobe | tee live_subs.txt
```
🔹 **What it does:**  
- Uses `subfinder` to enumerate subdomains.  
- Saves only new results using `anew`.  
- Checks if the subdomains are live using `httprobe`.  
- Stores the results in `live_subs.txt`.  

---

### **2️⃣ Find Open Ports with Masscan & Nmap**  
```bash
masscan -p1-65535 --rate=1000 -e tun0 --open -iL live_subs.txt -oG masscan_output.txt && cat masscan_output.txt | grep open | cut -d ' ' -f2 | anew open_ports.txt && nmap -p$(cat open_ports.txt | tr '\n' ',' | sed 's/,$//') -sV -sC -iL live_subs.txt -oN nmap_scan.txt
```
🔹 **What it does:**  
- Runs `masscan` for fast port scanning.  
- Extracts open ports and stores them in `open_ports.txt`.  
- Runs `nmap` on the found open ports for deeper service enumeration.  

---

### **3️⃣ Crawling for Hidden Endpoints (JS Files & APIs)**  
```bash
katana -u https://target.com -js -o js_files.txt && cat js_files.txt | grep -Eo 'https?://[^ ]+\.js' | anew all_js.txt && subjs < all_js.txt | tee endpoints.txt
```
🔹 **What it does:**  
- Uses `katana` to crawl the website and extract JS files.  
- Filters and stores JS files in `all_js.txt`.  
- Extracts endpoints from JavaScript files using `subjs`.  

---

### **4️⃣ Find Admin Panels & Sensitive Directories**  
```bash
gobuster dir -u https://target.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 50 -o gobuster_output.txt
```
🔹 **What it does:**  
- Uses `gobuster` to brute-force directories and admin panels.  
- Uses a common wordlist to find hidden paths.  

---

### **5️⃣ SQL Injection Testing on URLs**  
```bash
cat urls.txt | gf sqli | sqlmap --batch --random-agent --level 5 --risk 3 --dbs
```
🔹 **What it does:**  
- Extracts URLs with SQLi parameters using `gf sqli`.  
- Runs `sqlmap` in **batch mode** to detect SQL injection vulnerabilities.  

---

### **6️⃣ Takeover Detection for Subdomains**  
```bash
subfinder -d target.com | httpx -silent | subjack -w - -t 50 -o takeover.txt
```
🔹 **What it does:**  
- Finds subdomains with `subfinder`.  
- Checks live ones using `httpx`.  
- Detects subdomain takeover possibilities with `subjack`.  

---

### **7️⃣ Extract Secrets from JavaScript Files**  
```bash
cat js_files.txt | while read url; do curl -s "$url" | grep -Eo '"(apikey|token|secret|password)":\s*"[^"]+"' ; done
```
🔹 **What it does:**  
- Loops through JavaScript files.  
- Extracts potential API keys, tokens, secrets, or passwords.  

---

These are just a few **recon & vulnerability-hunting** one-liners! Let me know if you want **specific ones for SSRF, LFI, IDOR, or other vulnerabilities.** 🔥🚀
