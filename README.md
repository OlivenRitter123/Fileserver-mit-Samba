**Dokumentation: Einrichtung eines Samba-Fileservers**

**Systemumgebung:**

- Ein Raspberry Pi unter Linux wurde verwendet.
  
- Netzwerkzugang war gegeben.
  

---

### 1. **Installation von Samba**

Samba und die zugehörigen Pakete wurden installiert:

```
sudo apt update
sudo apt install samba
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

### 7. Backup eines Windows-Ordners auf den Samba-Share

**1. Samba-Share auf Windows verbinden**

Öffne den Datei-Explorer in Windows.

Gib in die Adressleiste die Netzwerkadresse des Samba-Shares ein, z. B.:

`\\192.168.142.141\shared`

Gib die Samba-Zugangsdaten ein (Benutzername und Passwort).

**2. Backup-Skript erstellen (Robocopy)**

Ein Batch-Skript wurde erstellt, um ein Backup des Windows-Ordners auf den Samba-Share durchzuführen:

```
@echo off
:: Samba-Login vor dem Backup herstellen
net use \\192.168.142.141\shared /user:olive mySecurePassword

:: Quell- und Zielordner definieren
set SOURCE="C:\Users\olive\Dokumente\3CHIT"
set DESTINATION="\\\\192.168.142.141\\shared"

:: Backup durchführen
robocopy %SOURCE% %DESTINATION% /MIR /FFT /Z /XA:H /W:5 /R:3

:: Samba-Verbindung trennen
net use \\192.168.142.141\shared /delete
```

net use: Baut die Verbindung mit den Zugangsdaten auf.

robocopy: Kopiert Dateien vom Windows-Ordner zum Samba-Share.

/delete: Trennt die Verbindung nach Abschluss des Backups.

**3. Automatisierung mit Aufgabenplanung (Task Scheduler)**

Das Skript wurde mit der Windows-Aufgabenplanung automatisiert:

Öffne die Aufgabenplanung.

Erstelle eine neue Aufgabe:

Gib der Aufgabe einen Namen (z. B. "Backup zu Samba-Share").

Wähle aus, dass sie täglich oder zu bestimmten Zeiten ausgeführt wird.

Verknüpfe das Skript (backup.bat) unter "Aktionen".
