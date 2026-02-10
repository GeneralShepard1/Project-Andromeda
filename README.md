# Service Enumeration, Web Recon & Security Checks

Ovaj dokument sadrži komande za provjeru running servisa, update statusa sistema, korisničke sigurnosti, web rekognosciranje i osnovne web sigurnosne provjere.

---

## 3. Local System Checks (Kali)

### 3.1 Lista running servisa na Kali

```bash
systemctl list-units --type=service --state=running
```

Ranjivo:
- nepotrebni servisi aktivni

Fix:
- stop / disable nepotrebne servise

---

### 3.2 Ko drži port (dokaz za Word)

```bash
sudo lsof -i -P -n | grep LISTEN
```

Prikazuje proces koji sluša na određenom portu.

---

### 3.3 Update status (patch stanje)

```bash
apt list --upgradable
```

Provjera dostupnih sigurnosnih update-a.

---

### 3.4 User sigurnost (lokalno)

```bash
who
w
sudo -l
cut -d: -f1 /etc/passwd
```

Provjera:
- aktivnih korisnika
- sudo privilegija
- liste korisničkih naloga

---

## 4. Domain & Network Recon

### Provjera dostupnosti domena

```bash
ping -c 2 www.xxx.com
nslookup www.xxx.com
dig www.xxx.com
```

Traži se:
- IP adresa domena
- da li domen odgovara
- više IP adresa (CDN / load balancer)

---

### 4.1 IP + geolokacija + ASN (bonus)

```bash
host www.xxx.com
dig +short www.xxx.com
whois www.xxx.com
```

Dobijaju se informacije o vlasniku domena i mreži.

---

### 4.2 Tehnologije sajta (framework, server, CMS)

```bash
whatweb http://www.xxx.com
whatweb https://www.xxx.com
```

Česte ranjivosti:
- stari WordPress / Joomla / Drupal
- stari Apache ili Nginx
- otkriveni headeri (Server, X-Powered-By)

Mitigacija:
- update CMS i pluginova
- sakriti nepotrebne headere
- hardening konfiguracije

---

### Brzi servis scan

```bash
sudo nmap -sS -sV -Pn -T4 <IP>
```

Traži se:
- da li je HTTPS aktivan
- portovi 8080 / 8443 (često admin paneli)

---

## 5. TLS & Certificate Checks

### 5.1 TLS verzije i cipheri

```bash
nmap --script ssl-enum-ciphers -p 443 www.xxx.com
```

Traže se stare TLS verzije i slabi cipheri.

---

### 5.2 Certifikat detalji

```bash
echo | openssl s_client -connect www.xxx.com:443 -servername www.xxx.com
```

Prikazuje detalje SSL certifikata.

---

## 6. HTTP Security Headers

```bash
curl -I http://www.xxx.com
curl -I https://www.xxx.com
```

Dobri headeri:
- Strict-Transport-Security
- Content-Security-Policy
- X-Frame-Options
- X-Content-Type-Options
- Referrer-Policy
- Permissions-Policy

Ako nedostaju → misconfiguration vulnerability.

Mitigacija:
- dodati security headers na web serveru
- CSP za zaštitu od XSS
- X-Frame-Options za sprječavanje clickjackinga

---

## 7. Nikto Scan (automatske web slabosti)

```bash
nikto -h http://www.xxx.com
nikto -h https://www.xxx.com
```

Nikto pronalazi:
- loše konfiguracije
- poznate opasne fajlove
- directory listing
- information disclosure

Mitigacija:
- update servera
- ukloniti testne stranice
- disable directory listing
- patch sistemi

---

## 8. Directory / File Brute-force

```bash
gobuster dir -u https://www.xxx.com/ -w /usr/share/wordlists/dirb/common.txt -k
gobuster dir -u http://www.xxx.com/ -w /usr/share/wordlists/dirb/common.txt
gobuster dir -u https://www.xxx.com/ -w /usr/share/wordlists/dirb/common.txt -k -b 403,404
```

Koristi se za pronalazak skrivenih direktorija i fajlova.

---

## 9. Robots & Sitemap (info leak)

```bash
curl -s https://www.xxx.com/robots.txt
curl -s https://www.xxx.com/sitemap.xml
```

Mogu otkriti skrivene ili administrativne putanje.

---

## 10. API Testovi (ako postoji /api ili swagger)

Provjera dostupnih endpointa i autentikacije.

---

## 11. SQLi / XSS (osnovna provjera)

```bash
sqlmap -u "https://www.xxx.com/products?id=5" --batch --risk=1 --level=1
```

Osnovna provjera SQL injection ranjivosti.

---

## 12. Auth slabosti

```bash
curl -I https://www.xxx.com/login
```

Provjera:
- redirecta
- security headera
- autentikacijskih mehanizama
