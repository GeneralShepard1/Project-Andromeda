3.1 Lista running servisa na Kali
systemctl list-units --type=service --state=running
Ranjivo: nepotrebni servisi upaljeni
Fix: stop/disable

3.2 Ko drži port (dokaz za Word)
sudo lsof -i -P -n | grep LISTEN

3.3 Update status (patch stanje)
apt list --upgradable

3.4 User sigurnost (lokalno)
who
w
sudo -l
cut -d: -f1 /etc/passwd


ping -c 2 www.xxx.com
nslookup www.xxx.com
dig www.xxx.com
Tražiš:
IP adresu domena
da li domen radi
da li ima više IP-ova (CDN / load balancer)

1.1 IP + geolokacija + ASN (bonus)
host www.xxx.com
dig +short www.xxx.com
whois www.xxx.com

1.2 Tehnologije sajta (framework, server, CMS)
whatweb http://www.xxx.com
whatweb https://www.xxx.com
Ranjivosti koje se često vide ovdje:
stari WordPress / Joomla / Drupal
stari Apache/Nginx
otkriveni headeri (Server, X-Powered-By)
Mitigacija:
update CMS + pluginova
sakriti nepotrebne headere
hardening konfiguracije

sudo nmap -sS -sV -Pn -T4 <IP>
Šta tražiš:
da li je HTTPS aktivan
da li postoji 8080/8443 (admin paneli često)

3.1 TLS verzije i cipheri
nmap --script ssl-enum-ciphers -p 443 www.xxx.com

3.2 Certifikat detalji
echo | openssl s_client -connect www.xxx.com:443 -servername www.xxx.com

4) HTTP Security Headers (brzo + puno bodova) (5 min)
curl -I http://www.xxx.com
curl -I https://www.xxx.com
Dobri headeri:
Strict-Transport-Security
Content-Security-Policy
X-Frame-Options
X-Content-Type-Options
Referrer-Policy
Permissions-Policy
Ako fali → vulnerability (misconfiguration)
Mitigacija (copy u Word):
dodati security headers na web serveru (nginx/apache)
CSP da blokira XSS
X-Frame-Options da spriječi clickjacking

Nikto (automatske web slabosti) (5–10 min)
nikto -h http://www.xxx.com
nikto -h https://www.xxx.com
Nikto hvata:
loše konfiguracije
poznate opasne fajlove
directory listing
info disclosure
Mitigacija:
update server
ukloniti testne stranice
disable listing
patch

6) Directory / file brute-force (najviše bodova) (10–15 min)
7) gobuster dir -u https://www.xxx.com/ -w /usr/share/wordlists/dirb/common.txt -k
gobuster dir -u http://www.xxx.com/ -w /usr/share/wordlists/dirb/common.txt
gobuster dir -u https://www.xxx.com/ -w /usr/share/wordlists/dirb/common.txt -k -b 403,404

7) Robots + sitemap (info leak) (2 min)
8) curl -s https://www.xxx.com/robots.txt
curl -s https://www.xxx.com/sitemap.xml

8) API testovi (ako vidiš /api ili swagger) (5–10 min)
9)  9) SQLi / XSS (samo osnovna provjera, bez “hakovanja”)
sqlmap -u "https://www.xxx.com/products?id=5" --batch --risk=1 --level=1

Auth slabosti
curl -I https://www.xxx.com/login



