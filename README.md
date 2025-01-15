**Dokumentation: Einrichtung eines Samba-Fileservers**

**Systemumgebung:**

- Ein Raspberry Pi unter Linux wurde verwendet.
  
- Netzwerkzugang war gegeben.
  

---

### 1. **Installation von Samba**

Samba und die zugehörigen Pakete wurden installiert:

```
sudo apt updatesudo apt install samba
```

Die Installation wurde mit folgendem Befehl überprüft:

```
smbd --version
```

---

### 2. **Konfiguration des Samba-Servers**

Die Konfigurationsdatei `/etc/samba/smb.conf` wurde bearbeitet und folgendes hinzugefügt:

```
[public]
path = /home/share
public = yes
writable = yes
force create mode = 0777
force directory mode = 0777
browsable = yes
```

Die Datei wurde gespeichert und geschlossen.

---

### 3. **Erstellen des Freigabeordners**

Der Freigabeordner wurde erstellt:

```
mkdir -p /home/share
```

Berechtigungen wurden wie folgt gesetzt:

```
sudo chown olive:olive /home/sharesudo chmod 777 /home/share
```

---

### 4. **Benutzer für Samba anlegen**

Ein Samba-Benutzerkonto für den Linux-Benutzer `olive` wurde erstellt:

```
sudo smbpasswd -a olive
```

Ein Passwort wurde festgelegt.

---

### 5. **Samba-Dienst starten und aktivieren**

Der Samba-Dienst wurde neu gestartet und aktiviert:

```
sudo systemctl restart smbdsudo systemctl enable smbd
```

Die Funktionsfähigkeit wurde wie folgt überprüft:

```
sudo systemctl status smbd
```

---

### 6. **Zugriff über das Netzwerk**

Der Zugriff auf die Freigabe erfolgte erfolgreich über folgende Schritte:

- **Windows:**
  
  1. Datei-Explorer geöffnet.
    
  2. `\\192.168.142.141\shared` in der Adressleiste eingegeben.
    
  3. Samba-Benutzername und Passwort eingegeben.
    
- **Linux:**
  
  1. Dateimanager geöffnet.
    
  2. Verbindung über `smb://192.168.142.141/shared` hergestellt.
