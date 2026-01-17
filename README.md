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
