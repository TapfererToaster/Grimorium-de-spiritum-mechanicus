# Projektübersicht

**Titel:**

> **Automatisierte Bereitstellung eines WordPress-Servers mit Nginx, PHP-FPM und PostgreSQL mittels Ansible**

**Zielsetzung:**  
Ziel des Projekts ist die Entwicklung einer modularen Ansible-Playbook-Struktur, die einen vollständigen WordPress-Server automatisiert bereitstellt.  
Dazu gehören die Installation und Konfiguration von Nginx, PHP-FPM, PostgreSQL und WordPress einschließlich Datenbankeinrichtung, SSL-Zertifikaten und grundlegender Sicherheitsmaßnahmen.

Das Projekt soll zeigen, wie sich mit Ansible eine reproduzierbare und idempotente Serverbereitstellung realisieren lässt.

## 🎯 **Projektziele und Ergebnisse**

|Ziel|Beschreibung|
|---|---|
|**Automatisierung**|Vollständige Automatisierung der Serverinstallation und -konfiguration über Ansible|
|**Rollenstruktur**|Saubere Trennung in wiederverwendbare Ansible-Rollen (nginx, php, postgresql, wordpress)|
|**Variablenmanagement**|Verwendung von Variablen, Templates und Ansible-Vault für sensible Daten|
|**Dokumentation**|Vollständige technische Dokumentation mit Playbook-Struktur, Variablenbeschreibung und Ablauf|
|**Testumgebung**|Lokale Testumgebung mit Vagrant oder Docker für Wiederholbarkeit|
|**Deployment-Ergebnis**|Nach Ausführung: Ein laufender WordPress-Server mit PostgreSQL-Datenbank und Nginx-Webserver|

---

## ⏱️ **Zeitplan (ca. 40 Stunden Gesamt)**

| Phase                                | Aufgabe                                                                                                                                               | Zeit (h) | Ergebnis                                                      |
| ------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------------------------------------------------------------- |
| **1. Planung & Vorbereitung**        | - Zieldefinition  <br>- Auswahl der Softwareversionen  <br>- Aufbau des Ansible-Projektverzeichnisses                                                 | 4        | Grundstruktur steht (`inventory/`, `roles/`, `playbooks/`)    |
| **2. Rollenerstellung – Nginx**      | - Installation von Nginx  <br>- Erstellung einer Jinja2-Template-Datei für Virtual Host  <br>- Handler zum Neustart des Dienstes                      | 5        | Rolle `nginx` mit `tasks/main.yml`, `templates/nginx.conf.j2` |
| **3. Rollenerstellung – PHP-FPM**    | - Installation & Konfiguration von PHP-FPM  <br>- Anpassung `php.ini` per Template                                                                    | 4        | Rolle `php`                                                   |
| **4. Rollenerstellung – PostgreSQL** | - Installation und DB-Setup  <br>- Benutzer & Passwort via Variablen  <br>- Verwendung von `ansible-vault` für Secrets                                | 5        | Rolle `postgresql` mit Datenbank & User                       |
| **5. Rollenerstellung – WordPress**  | - Download von WordPress  <br>- Deployment ins Webverzeichnis  <br>- Template für `wp-config.php` (mit PostgreSQL-Treiber)                            | 6        | Rolle `wordpress`                                             |
| **6. Integration & Testing**         | - Kombinierte Ausführung aller Rollen in einem Playbook  <br>- Idempotenztests (mehrfaches Ausführen ohne Änderungen)  <br>- Tests mit `ansible-lint` | 5        | Voll funktionsfähiger Stack                                   |
| **7. Absicherung & Optimierung**     | - Installation von Certbot (SSL)  <br>- Firewall (ufw)  <br>- File-Rechte für `/var/www/wordpress`                                                    | 4        | HTTPS aktiviert, Server abgesichert                           |
| **8. Dokumentation**                 | - README.md mit Installationsanleitung  <br>- Diagramm der Infrastruktur  <br>- Beschreibung der Rollen und Variablen                                 | 5        | Fertige technische Dokumentation                              |

---

## 🧱 **Projektstruktur (Beispiel)**

```
wordpress-ansible/
├── inventory/
│   └── hosts.yml
├── playbooks/
│   └── site.yml
├── roles/
│   ├── nginx/
│   │   ├── tasks/main.yml
│   │   ├── templates/nginx.conf.j2
│   │   └── defaults/main.yml
│   ├── php/
│   │   ├── tasks/main.yml
│   └── postgresql/
│       ├── tasks/main.yml
│       └── templates/pg_hba.conf.j2
│   └── wordpress/
│       ├── tasks/main.yml
│       ├── templates/wp-config.php.j2
│       └── files/pg4wp/
├── group_vars/
│   └── all.yml
└── README.md
```

---

## 🔐 **Beispiel: Variablen und Secrets**

**`group_vars/all.yml`:**
```
wp_db_name: wordpress
wp_db_user: wpuser
wp_db_password: "{{ vault_wp_db_password }}"
wp_url: "example.com"
nginx_root: "/var/www/wordpress"
```

**`vault.yml` (verschlüsselt mit `ansible-vault`):**
```
`vault_wp_db_password: "SuperGeheimesPasswort"`
```

---

## 🧪 **Testumgebung**

- **Option 1:** Vagrant + VirtualBox  
    → schnelles lokales Testen mit `vagrant up`
    
- **Option 2:** Docker + Ansible Local Connection  
    → leichtgewichtig und CI/CD-tauglich
    

---

## 📈 **Erweiterungsmöglichkeiten (optional, falls Zeit bleibt)**

|Idee|Nutzen|
|---|---|
|**Molecule-Tests**|Automatisierte Überprüfung der Rollen|
|**Load Balancer hinzufügen**|Zweiter Webserver + Nginx Load Balancer|
|**CI/CD Integration**|Automatische Ausführung via GitLab/GitHub Actions|
|**Monitoring mit Prometheus**|Erweiterung um Systemmetriken|

---

## 🧾 **Abschluss & Ergebnis**

Nach Projektabschluss steht:

- Ein **funktionierender WordPress-Server** (Nginx, PHP, PostgreSQL)
    
- Vollständig **automatisierte Provisionierung** über Ansible
    
- **Strukturierte Rollen & Templates**
    
- **Dokumentation** inkl. Anleitung zur Wiederholung
    

Das Projekt ist praxisnah, realistisch und deckt zentrale DevOps-Kompetenzen ab:

- Infrastructure as Code
    
- Automatisierung
    
- Konfigurationsmanagement
    
- Deployment-Pipelines


👉 **Projektidee 2: Multi-Server Deployment mit Load Balancer, Webservern und Datenbank (Ansible)**

Das Projekt ist praxisnah, etwa **40 Stunden** Umfang, und zeigt fortgeschrittene Ansible-Kenntnisse (Rollen, Multi-Host-Inventories, Abhängigkeiten, Load Balancing).

## Step-by-step configuration

Here’s a step-by-step guide to configure a **server stack with Nginx, PostgreSQL, and WordPress**.  
This setup is commonly referred to as an **LEMP stack (Linux, Nginx, PostgreSQL, PHP)** variant — since WordPress is traditionally MySQL-based, a few extra steps are needed to use PostgreSQL.

---

## 🧩 Overview of Components

|Component|Role|
|---|---|
|**Nginx**|Web server handling HTTP requests and serving WordPress content|
|**PHP-FPM**|Executes PHP code for WordPress|
|**PostgreSQL**|Database storing WordPress content and settings|
|**WordPress**|CMS (Content Management System)|

---

## ⚙️ 1. Prepare the Server

Assume you’re using **Ubuntu/Debian** (other distros similar):

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install nginx php-fpm php-pgsql php-cli php-mbstring php-xml php-curl php-zip -y
sudo apt install postgresql postgresql-contrib -y
```
---

## 🧱 2. Configure PostgreSQL

1. **Switch to the `postgres` user and create a database and user:**
    
    `sudo -u postgres psql`
    
2. **Inside the psql shell:**
```sql
CREATE DATABASE wordpress;
CREATE USER wpuser WITH ENCRYPTED PASSWORD 'strongpassword';
GRANT ALL PRIVILEGES ON DATABASE wordpress TO wpuser;
\q
```
    
3. **Adjust PostgreSQL to listen for WordPress connections if needed:**
    
    ```bash
    sudo nano /etc/postgresql/*/main/pg_hba.conf
    ```
    
    Change the local authentication to:
    
    ```sql
    local   all             all                                     md5
    ```
    
4. **Restart PostgreSQL:**
    
    ```bash
    sudo systemctl restart postgresql
    ```
    

---

## 🌐 3. Install and Configure WordPress

### A. Download WordPress

```
cd /var/www/
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xzf latest.tar.gz
sudo mv wordpress /var/www/wordpress
sudo chown -R www-data:www-data /var/www/wordpress
sudo chmod -R 755 /var/www/wordpress
```

### B. Enable PostgreSQL Support for WordPress

WordPress does **not** natively support PostgreSQL. You’ll need the **PostgreSQL for WordPress (PG4WP)** plugin.

1. **Install the PG4WP driver:**
```
cd /var/www/wordpress/wp-content
mkdir -p db
cd db
wget https://github.com/kevinoid/wp-postgresql/archive/refs/heads/master.zip
unzip master.zip
mv wp-postgresql-master/* .
rm -rf wp-postgresql-master master.zip
```
    
2. **Edit `wp-config.php` later (see below) to point to PostgreSQL.**
    

---

## 🛠️ 4. Configure Nginx for WordPress

Create a new site config:
```bash
sudo nano /etc/nginx/sites-available/wordpress
```
Add:

```nginx
server {
    listen 80;
    server_name example.com;
    root /var/www/wordpress;

    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php-fpm.sock;
    }

    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
        expires max;
        log_not_found off;
    }

    access_log /var/log/nginx/wordpress_access.log;
    error_log /var/log/nginx/wordpress_error.log;
}
```
Enable and test:

```bash
sudo ln -s /etc/nginx/sites-available/wordpress /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

---

## ✏️ 5. Configure WordPress Connection

1. **Copy the sample config:**
    
```bash
cd /var/www/wordpress
cp wp-config-sample.php wp-config.php
```
    
2. **Edit `wp-config.php`:**
    
    ```bash
    sudo nano wp-config.php
    ```
    
    Replace MySQL settings with:
    
```php
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'wpuser' );
define( 'DB_PASSWORD', 'strongpassword' );
define( 'DB_HOST', 'localhost' );
define( 'DB_DRIVER', 'pgsql' ); // Required for PG4WP

// Include PostgreSQL compatibility
require_once( ABSPATH . 'wp-content/db/db.php' );
```
    
    Add your WordPress salts (you can generate from https://api.wordpress.org/secret-key/1.1/salt/).
    

---

## 🚀 6. Complete Installation in Browser

Visit:

`http://your-server-ip/`

You should see the WordPress setup wizard. Follow the steps to finalize installation.

---

## 🔒 7. (Optional) Secure and Optimize

- Install **Certbot** for HTTPS:
    
    `sudo apt install certbot python3-certbot-nginx -y sudo certbot --nginx -d example.com`
    
- Enable automatic updates and backups.
    
- Tune `php.ini` for performance (upload size, memory limit, etc.).
    
- Consider caching plugins or Nginx FastCGI cache.
    

---

## ✅ Summary

|Step|Description|
|---|---|
|1|Install Nginx, PHP-FPM, and PostgreSQL|
|2|Configure PostgreSQL database and user|
|3|Download WordPress and add PG4WP|
|4|Set up Nginx virtual host|
|5|Edit `wp-config.php` for PostgreSQL|
|6|Run the web installer|
|7|Secure and optimize the stack|

---

Would you like me to provide a **ready-to-run setup script** (Bash) that automates these steps?
# 🧭 Projektplan  
## Automatisierte Bereitstellung eines WordPress-Stacks mit Nginx, PHP-FPM und PostgreSQL mittels Ansible

---

### 📋 Projektübersicht

**Titel:**  
Automatisierte Bereitstellung eines WordPress-Servers mit Nginx, PHP-FPM und PostgreSQL mittels Ansible

**Zielsetzung:**  
Ziel des Projekts ist die Entwicklung einer modularen Ansible-Playbook-Struktur, die einen vollständigen WordPress-Server automatisiert bereitstellt.  
Dazu gehören die Installation und Konfiguration von Nginx, PHP-FPM, PostgreSQL und WordPress einschließlich Datenbankeinrichtung, SSL-Zertifikaten und grundlegender Sicherheitsmaßnahmen.  

Das Projekt soll zeigen, wie sich mit Ansible eine reproduzierbare und idempotente Serverbereitstellung realisieren lässt.

---

### 🎯 Projektziele und Ergebnisse

| Ziel | Beschreibung |
|------|---------------|
| **Automatisierung** | Vollständige Automatisierung der Serverinstallation und -konfiguration über Ansible |
| **Rollenstruktur** | Saubere Trennung in wiederverwendbare Ansible-Rollen (nginx, php, postgresql, wordpress) |
| **Variablenmanagement** | Verwendung von Variablen, Templates und Ansible-Vault für sensible Daten |
| **Dokumentation** | Vollständige technische Dokumentation mit Playbook-Struktur, Variablenbeschreibung und Ablauf |
| **Testumgebung** | Lokale Testumgebung mit Vagrant oder Docker für Wiederholbarkeit |
| **Deployment-Ergebnis** | Nach Ausführung: Ein laufender WordPress-Server mit PostgreSQL-Datenbank und Nginx-Webserver |

---

### ⏱️ Zeitplan (ca. 40 Stunden Gesamt)

| Phase | Aufgabe | Zeit (h) | Ergebnis |
|--------|----------|-----------|-----------|
| **1. Planung & Vorbereitung** | - Zieldefinition<br>- Auswahl der Softwareversionen<br>- Aufbau des Ansible-Projektverzeichnisses | 4 | Grundstruktur steht (`inventory/`, `roles/`, `playbooks/`) |
| **2. Rollenerstellung – Nginx** | - Installation von Nginx<br>- Erstellung einer Jinja2-Template-Datei für Virtual Host<br>- Handler zum Neustart des Dienstes | 5 | Rolle `nginx` mit `tasks/main.yml`, `templates/nginx.conf.j2` |
| **3. Rollenerstellung – PHP-FPM** | - Installation & Konfiguration von PHP-FPM<br>- Anpassung `php.ini` per Template | 4 | Rolle `php` |
| **4. Rollenerstellung – PostgreSQL** | - Installation und DB-Setup<br>- Benutzer & Passwort via Variablen<br>- Verwendung von `ansible-vault` für Secrets | 5 | Rolle `postgresql` mit Datenbank & User |
| **5. Rollenerstellung – WordPress** | - Download von WordPress<br>- Deployment ins Webverzeichnis<br>- Template für `wp-config.php` (mit PostgreSQL-Treiber) | 6 | Rolle `wordpress` |
| **6. Integration & Testing** | - Kombinierte Ausführung aller Rollen in einem Playbook<br>- Idempotenztests (mehrfaches Ausführen ohne Änderungen)<br>- Tests mit `ansible-lint` | 5 | Voll funktionsfähiger Stack |
| **7. Absicherung & Optimierung** | - Installation von Certbot (SSL)<br>- Firewall (ufw)<br>- File-Rechte für `/var/www/wordpress` | 4 | HTTPS aktiviert, Server abgesichert |
| **8. Dokumentation** | - README.md mit Installationsanleitung<br>- Diagramm der Infrastruktur<br>- Beschreibung der Rollen und Variablen | 5 | Fertige technische Dokumentation |

## 🧩 PHP-Pakete erklärt

|Paket|Funktion|Warum für WordPress wichtig|
|---|---|---|
|**php-fpm**|_PHP FastCGI Process Manager_ – führt PHP-Skripte aus und kommuniziert mit Nginx über FastCGI.|Nginx selbst kann kein PHP verarbeiten – dafür braucht man php-fpm, um WordPress-Seiten zu rendern.|
|**php-pgsql**|Stellt PHP-Funktionen für den Zugriff auf **PostgreSQL-Datenbanken** bereit.|Ohne dieses Modul kann WordPress (bzw. das PG4WP-Plugin) keine Verbindung zur PostgreSQL-Datenbank herstellen.|
|**php-cli**|Ermöglicht die Ausführung von PHP im Terminal (Command Line Interface).|Wird für WP-CLI (WordPress Command Line Interface), Skripte und Wartungsaufgaben benötigt.|
|**php-mbstring**|Bietet Unterstützung für **Multibyte-Strings** (z. B. UTF-8).|WordPress verwendet UTF-8-Zeichen, also wichtig für internationale Texte, E-Mails und Plugins.|
|**php-xml**|Fügt Funktionen für XML-Verarbeitung hinzu.|WordPress braucht es für den RSS-Feed, Import/Export-Funktionen und viele Plugins.|
|**php-curl**|Ermöglicht HTTP-Anfragen über PHP (Client URL Library).|WordPress nutzt es für Updates, API-Aufrufe und externe Dienste (z. B. Plugin-Updates).|
|**php-zip**|Bietet ZIP-Archiv-Unterstützung.|Wird benötigt, um Themes/Plugins direkt im Dashboard hochzuladen oder Updates zu entpacken.|



# 🧭 Projektplan  
## Automatisierte Bereitstellung einer Multi-Server-Infrastruktur mit Ansible (Load Balancer, Webserver, Datenbank)

---

### 📋 Projektübersicht

**Titel:**  
Automatisierte Bereitstellung einer Multi-Server-Infrastruktur mit Load Balancer, Webservern und Datenbank mittels Ansible

**Zielsetzung:**  
Ziel dieses Projekts ist die Entwicklung einer modularen Ansible-Struktur, die eine komplette, mehrschichtige Infrastruktur automatisiert bereitstellt.  
Die Umgebung besteht aus:
- **1 Load Balancer (HAProxy oder Nginx)**
- **2 Webservern (Nginx + PHP-FPM)**
- **1 Datenbankserver (PostgreSQL)**  

Alle Server werden automatisch konfiguriert, verbunden und getestet.  
Das Projekt demonstriert, wie Ansible für orchestrierte Deployments über mehrere Hosts hinweg genutzt wird.

---

### 🎯 Projektziele und Ergebnisse

| Ziel | Beschreibung |
|------|---------------|
| **Multi-Host-Orchestrierung** | Automatisiertes Provisionieren mehrerer Serverrollen mit Ansible |
| **Skalierbarkeit** | Load Balancer kann mehrere Webserver ansprechen |
| **Idempotenz** | Mehrfaches Ausführen der Playbooks darf keine ungewollten Änderungen erzeugen |
| **Variablen- und Rollenstruktur** | Klare Trennung von Hostgruppen und Rollen |
| **Wiederholbarkeit** | Gleiche Umgebung kann reproduzierbar aufgesetzt werden |
| **Dokumentation** | Vollständige technische Dokumentation und Ablaufbeschreibung |

---

### 🖥️ Infrastrukturübersicht

```text
                 ┌──────────────────┐
                 │   Load Balancer  │
                 │  (HAProxy/Nginx) │
                 └──────┬───────────┘
                        │
        ┌───────────────┴───────────────┐
        │                               │
 ┌───────────────┐              ┌───────────────┐
 │   Webserver 1  │              │   Webserver 2  │
 │ (Nginx + PHP)  │              │ (Nginx + PHP)  │
 └───────────────┘              └───────────────┘
        │                               │
        └───────────────┬───────────────┘
                        │
                ┌───────────────┐
                │  Datenbank     │
                │ (PostgreSQL)   │
                └───────────────┘
```


### ⏱️ Zeitplan (ca. 40 Stunden Gesamt)

|Phase|Aufgabe|Zeit (h)|Ergebnis|
|---|---|---|---|
|**1. Planung & Architektur**|- Design der Zielarchitektur  <br>- Erstellung der Host-Gruppen im Inventory  <br>- Definition der Rollenstruktur|4|Grundlegendes Infrastruktur-Design steht|
|**2. Rollenerstellung – Load Balancer**|- Installation HAProxy oder Nginx  <br>- Konfiguration per Template (Backend-Server aus Inventory)  <br>- Healthchecks einbauen|6|Rolle `loadbalancer` fertig|
|**3. Rollenerstellung – Webserver**|- Installation Nginx & PHP-FPM  <br>- Deployment einer Beispielseite (z. B. PHP-Testseite oder Mini-App)  <br>- Handler für Neustarts|6|Rolle `webserver` fertig|
|**4. Rollenerstellung – Datenbank (PostgreSQL)**|- Installation PostgreSQL  <br>- Einrichtung von DB & Benutzer  <br>- Anpassung von `pg_hba.conf` für Remote-Zugriffe|5|Rolle `database` fertig|
|**5. Integration & Inventory-Management**|- Anlegen von `group_vars` & `host_vars`  <br>- Nutzung von Variablen pro Gruppe (z. B. Webserver vs. Load Balancer)  <br>- Template-Integration|5|Sauber getrennte Konfiguration|
|**6. Testing & Debugging**|- Test der gesamten Playbooks  <br>- Idempotenzprüfungen  <br>- Service-Connectivity-Tests (Load Balancer ↔ Webserver ↔ DB)|6|Stabile Multi-Host-Umgebung|
|**7. Absicherung & Optimierung**|- Firewall (ufw) konfigurieren  <br>- SSL-Zertifikate über Let’s Encrypt (optional)  <br>- Logging & Systemhärtung|4|Sicherheitskonfiguration aktiv|
|**8. Dokumentation & Abschluss**|- Erstellung von README.md  <br>- Diagramme & Ablaufbeschreibung  <br>- Lessons Learned|4|Vollständige Projektdokumentation|

---

### 🧱 Projektstruktur (Beispiel)

```
multi-server-ansible/
├── inventory/
│   └── hosts.yml
├── playbooks/
│   └── site.yml
├── group_vars/
│   ├── all.yml
│   ├── loadbalancer.yml
│   ├── webservers.yml
│   └── database.yml
├── roles/
│   ├── loadbalancer/
│   │   ├── tasks/main.yml
│   │   ├── templates/haproxy.cfg.j2
│   │   └── defaults/main.yml
│   ├── webserver/
│   │   ├── tasks/main.yml
│   │   ├── templates/nginx.conf.j2
│   │   └── files/index.php
│   └── database/
│       ├── tasks/main.yml
│       └── templates/pg_hba.conf.j2
└── README.md
```
---

### 🗂️ Beispiel-Inventory (`inventory/hosts.yml`)

```
all:
  children:
    loadbalancer:
      hosts:
        lb01:
          ansible_host: 192.168.56.10
    webservers:
      hosts:
        web01:
          ansible_host: 192.168.56.11
        web02:
          ansible_host: 192.168.56.12
    database:
      hosts:
        db01:
          ansible_host: 192.168.56.13
```
---

### 🔐 Beispiel-Variablen

**`group_vars/webservers.yml`**

```
nginx_root: /var/www/html
app_name: demo_app
php_fpm_socket: /run/php/php-fpm.sock
```

**`group_vars/loadbalancer.yml`**
```
haproxy_backends:
  - name: web01
    address: 192.168.56.11
    port: 80
  - name: web02
    address: 192.168.56.12
    port: 80
```
**`group_vars/database.yml`**
```
db_name: appdb
db_user: appuser
db_password: "{{ vault_db_password }}"
```

---

### 🧪 Testumgebung

- **Vagrant + VirtualBox:**  
    Für einfache lokale Multi-VM-Tests (1 LB, 2 Web, 1 DB)
    
- **Docker Compose:**  
    Leichtgewichtiges Setup mit definierten Containern
    
- **Optional:**  
    Cloud-Testing via AWS EC2 oder DigitalOcean (mit dynamischem Inventory)
    

---

### 📈 Erweiterungsmöglichkeiten (optional)

|Idee|Nutzen|
|---|---|
|**Rolling Updates**|Deployment ohne Ausfallzeit|
|**Zero-Downtime Load Balancing**|HAProxy mit Sticky Sessions|
|**CI/CD-Integration**|Automatischer Rollout über GitLab/GitHub Actions|
|**Monitoring hinzufügen**|Prometheus, Grafana oder ELK Stack über zusätzliche Rollen|
|**HA-Datenbank**|Replikation mit PostgreSQL Streaming Replication|

---

### 🧾 Abschluss & Ergebnis

Nach Projektabschluss steht:

- Eine **voll funktionsfähige Multi-Server-Umgebung**  
    (Load Balancer → Webserver → PostgreSQL)
    
- Automatisierte Bereitstellung aller Komponenten über Ansible
    
- Wiederholbare, dokumentierte Infrastruktur
    
- Saubere Rollen- und Variablenstruktur
    

---

### 🧠 Erlernte Kompetenzen

- Multi-Host-Orchestrierung mit Ansible
    
- Nutzung von `group_vars` und `host_vars`
    
- Templates & Variablen für dynamische Konfiguration
    
- Load Balancing und Service-Abhängigkeiten
    
- Reproduzierbare Infrastruktur (Infrastructure as Code)
    
- Fehlersuche & Idempotenzkontrolle
    

---

**Autor:** _[Dein Name]_  
**Dauer:** 40 Stunden  
**Tools:** Ansible, Vagrant/Docker, Nginx/HAProxy, PostgreSQL, PHP-FPM  
**Lizenz:** MIT (optional)


## 🧭 Overview

### 🎯 Goal:

Automate the installation and configuration of:

- **Linux (Ubuntu/Debian)**
    
- **Apache**
    
- **MySQL/MariaDB**
    
- **PHP**
    
- **WordPress**
    

---

## 🧩 1. Directory Structure

Recommended project layout:

```css
lamp-wordpress/
├── inventory
│   └── hosts.ini
├── playbooks
│   └── site.yml
├── roles/
│   ├── apache/
│   │   └── tasks/main.yml
│   ├── mysql/
│   │   └── tasks/main.yml
│   ├── php/
│   │   └── tasks/main.yml
│   └── wordpress/
│       └── tasks/main.yml
└── group_vars/
    └── all.yml
```

---

## ⚙️ 2. Inventory File (`inventory/hosts.ini`)

Define your target server(s):

`[web] webserver ansible_host=192.168.56.10 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa`

---

## 🔐 3. Variables (`group_vars/all.yml`)

Store global settings and credentials:

```yaml
# Database credentials
db_name: wordpress_db
db_user: wordpress_user
db_password: strongpassword

# WordPress download URL
wp_version: latest
wp_download_url: https://wordpress.org/{{ wp_version }}.tar.gz

# Apache
apache_doc_root: /var/www/html/wordpress
```

---

## 🧰 4. Master Playbook (`playbooks/site.yml`)

This is the main entry point:

```yaml
---
- name: Setup LAMP + WordPress Stack
  hosts: web
  become: yes

  roles:
    - role: apache
    - role: mysql
    - role: php
    - role: wordpress
```

---

## 🔹 5. Apache Role (`roles/apache/tasks/main.yml`)

```yaml
---
- name: Install Apache
  apt:
    name: apache2
    state: present
    update_cache: yes

- name: Enable and start Apache service
  service:
    name: apache2
    state: started
    enabled: yes
```

---

## 🔹 6. MySQL Role (`roles/mysql/tasks/main.yml`)

```yaml
---
- name: Install MySQL server
  apt:
    name: mysql-server
    state: present

- name: Start and enable MySQL
  service:
    name: mysql
    state: started
    enabled: yes

- name: Create WordPress database
  mysql_db:
    name: "{{ db_name }}"
    state: present

- name: Create WordPress user
  mysql_user:
    name: "{{ db_user }}"
    password: "{{ db_password }}"
    priv: "{{ db_name }}.*:ALL"
    state: present
```

---

## 🔹 7. PHP Role (`roles/php/tasks/main.yml`)

```yaml
---
- name: Install PHP and required extensions
  apt:
    name:
      - php
      - libapache2-mod-php
      - php-mysql
      - php-cli
      - php-curl
      - php-xml
      - php-mbstring
      - php-zip
      - php-gd
    state: present
    update_cache: yes

- name: Restart Apache to load PHP module
  service:
    name: apache2
    state: restarted
```

---

## 🔹 8. WordPress Role (`roles/wordpress/tasks/main.yml`)

```yaml
---
- name: Download WordPress archive
  get_url:
    url: "{{ wp_download_url }}"
    dest: /tmp/wordpress.tar.gz

- name: Extract WordPress
  unarchive:
    src: /tmp/wordpress.tar.gz
    dest: /var/www/html/
    remote_src: yes

- name: Set ownership and permissions
  file:
    path: /var/www/html/wordpress
    owner: www-data
    group: www-data
    mode: '0755'
    recurse: yes

- name: Copy sample config
  command: cp /var/www/html/wordpress/wp-config-sample.php /var/www/html/wordpress/wp-config.php
  args:
    creates: /var/www/html/wordpress/wp-config.php

- name: Configure WordPress database settings
  lineinfile:
    path: /var/www/html/wordpress/wp-config.php
    regexp: "{{ item.regexp }}"
    line: "{{ item.line }}"
  with_items:
    - { regexp: "define\\( 'DB_NAME',", line: "define( 'DB_NAME', '{{ db_name }}' );" }
    - { regexp: "define\\( 'DB_USER',", line: "define( 'DB_USER', '{{ db_user }}' );" }
    - { regexp: "define\\( 'DB_PASSWORD',", line: "define( 'DB_PASSWORD', '{{ db_password }}' );" }
    - { regexp: "define\\( 'DB_HOST',", line: "define( 'DB_HOST', 'localhost' );" }

- name: Enable Apache rewrite module
  command: a2enmod rewrite
  notify: Restart Apache

- name: Create Apache virtual host
  copy:
    dest: /etc/apache2/sites-available/wordpress.conf
    content: |
      <VirtualHost *:80>
        DocumentRoot /var/www/html/wordpress
        <Directory /var/www/html/wordpress>
            AllowOverride All
        </Directory>
      </VirtualHost>

- name: Enable WordPress site
  command: a2ensite wordpress.conf

- name: Disable default site
  command: a2dissite 000-default.conf

- name: Reload Apache
  service:
    name: apache2
    state: reloaded
```

---

## 🧪 9. Run the Playbook

From your project root:

`ansible-playbook -i inventory/hosts.ini playbooks/site.yml`

Once complete, open a browser and go to:

`http://<server-ip>/`

You should see the **WordPress installation setup page** 🎉

---

## ✅ Verification Checklist

|Component|Check|Command|
|---|---|---|
|Apache|Running|`systemctl status apache2`|
|MySQL|Database created|`mysql -u root -p -e 'SHOW DATABASES;'`|
|PHP|Active module|`php -v`|
|WordPress|Accessible|Visit in browser|

---

## 🧠 Bonus: Improvements & Extensions

| Feature              | Description                                             |
| -------------------- | ------------------------------------------------------- |
| **SSL Setup**        | Add a Certbot role to enable HTTPS automatically        |
| **Firewall**         | Automate `ufw` setup for secure ports (22, 80, 443)     |
| **Separate DB Host** | Expand to multi-host environment (DB on another server) |
| **WP-CLI Role**      | Manage plugins and users through automation             |
| **Backups**          | Add scheduled DB dumps or rsync backups                 |

---

## 🧾 Summary

|Step|Purpose|
|---|---|
|Define Inventory & Variables|Target host and credentials|
|Create Roles (Apache, MySQL, PHP, WordPress)|Modular setup|
|Configure WordPress|Automated installation and DB link|
|Test and Verify|Ensure full functionality|
# Required PHP packages
- php
- php8.2-cli
- php-curl
- php8.2-gd
- php-json
- php-mbstring
- php-mysql
- php-xml
- php-zip
