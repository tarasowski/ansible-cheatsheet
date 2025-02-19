# Ansible Cheat Sheet (Deutsch)

## 1. Installation
```sh
# Auf Debian/Ubuntu
sudo apt update && sudo apt install ansible -y

# Auf CentOS/RHEL
sudo yum install epel-release -y
sudo yum install ansible -y

# Über Pip
pip install ansible
```

## 2. Inventar-Datei (hosts)
```ini
[webserver]
web01 ansible_host=192.168.1.10 ansible_user=root
web02 ansible_host=192.168.1.11 ansible_user=root

[datenbank]
db01 ansible_host=192.168.1.20 ansible_user=root
```

## 3. Ansible Befehle
```sh
# Verbindung testen
ansible all -m ping

# Befehl auf allen Hosts ausführen
ansible all -m command -a "uptime"

# Playbook ausführen
ansible-playbook mein_playbook.yml
```

## 4. Ad-hoc Befehle
```sh
# Datei auf alle Hosts kopieren
ansible all -m copy -a "src=/etc/hosts dest=/tmp/hosts"

# Paket installieren
ansible all -m apt -a "name=nginx state=present" -b

# Dienst starten
ansible all -m service -a "name=nginx state=started" -b
```

## 5. Playbook Beispiel (YAML)
```yaml
- name: Webserver einrichten
  hosts: webserver
  become: true
  tasks:
    - name: Nginx installieren
      apt:
        name: nginx
        state: present

    - name: Nginx starten
      service:
        name: nginx
        state: started
```

## 6. Variablen
```yaml
vars:
  nginx_port: 80

tasks:
  - name: Konfigurationsdatei bereitstellen
    template:
      src: nginx.conf.j2
      dest: /etc/nginx/nginx.conf
```

## 7. Rollen nutzen
```sh
# Neue Rolle erstellen
ansible-galaxy init meine_rolle
```
Verzeichnisstruktur einer Rolle:
```sh
meine_rolle/
│── tasks/
│   ├── main.yml
│── handlers/
│   ├── main.yml
│── templates/
│── files/
│── vars/
│── defaults/
│── meta/
```

## 8. Nützliche Ansible-Optionen
```sh
# Syntax überprüfen
ansible-playbook mein_playbook.yml --syntax-check

# Trockenlauf (ohne Änderungen)
ansible-playbook mein_playbook.yml --check

# Detaillierte Ausgabe
ansible-playbook mein_playbook.yml -v
```

## 9. Debugging
```yaml
- name: Debug Nachricht ausgeben
  debug:
    msg: "Der Wert von nginx_port ist {{ nginx_port }}"
```

## 10. Ansible Vault (Geheime Daten verschlüsseln)
```sh
# Neue Datei mit Ansible Vault erstellen
ansible-vault create geheim.yml

# Datei bearbeiten
ansible-vault edit geheim.yml

# Playbook mit verschlüsselten Variablen ausführen
ansible-playbook mein_playbook.yml --ask-vault-pass
