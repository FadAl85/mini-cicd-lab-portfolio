# Mini CI/CD Lab

## Überblick

Dieses Lab bildet einen vollständigen CI/CD-Ablauf in einer lokalen, kontrollierten Testumgebung ab.  
Die Anwendung ist bewusst klein gehalten, damit der Fokus auf der technischen Delivery-Kette liegt:

**Code → Test → Container → Registry → Kubernetes Deployment**

Ziel ist eine nachvollziehbare Umgebung, die zeigt, wie moderne Build-, Test- und Deployment-Prozesse zusammenspielen.

---

## Architektur

```text
Developer / VS Code
        |
        v
      Git
        |
        v
GitHub Repository
        |
        v
GitHub Actions Pipeline
        |
        +--------------------+
        |                    |
        v                    v
PowerShell Test        Docker Build
        |                    |
        +---------+----------+
                  |
                  v
      GitHub Container Registry
                  |
                  v
        Self-hosted Runner
                  |
                  v
        Kubernetes Deployment
                  |
        +---------+----------+
        |                    |
        v                    v
      Pod              Service / NodePort
        |
        v
   Web App Version V1 → V2
```

---

## Umgesetzt

- Lokale Entwicklungsumgebung mit Windows 11, WSL2, Docker Desktop, Git und VS Code
- Einfache Webanwendung als bewusst schlanker Test-Use-Case
- Containerisierung der Anwendung mit Docker und Nginx
- GitHub Repository mit sauberer Versionierung
- GitHub Actions Pipeline für automatisierte Verarbeitung
- PowerShell-Testskript zur technischen Prüfung der Anwendung
- Docker Image Build und Versionierung
- Veröffentlichung des Images über GitHub Container Registry
- Kubernetes Deployment mit Pod, Service und NodePort
- Einsatz von Kubernetes Secrets für sensible Konfigurationen
- Self-hosted Runner als Verbindung zwischen GitHub und lokaler Zielumgebung
- Erfolgreicher Deployment-Test inklusive Update von Version V1 auf Version V2

---

## Technischer Fokus

```text
Git / GitHub
GitHub Actions
PowerShell Testing
Docker / Nginx
GitHub Container Registry
Self-hosted Runner
Kubernetes Deployment
Pod / Service / NodePort
Kubernetes Secrets
```

---

## Projektstatus

Der Kernablauf des Labs ist umgesetzt:  
Eine Codeänderung kann versioniert, geprüft, containerisiert, veröffentlicht und in einer Kubernetes-Umgebung bereitgestellt werden.

Die öffentliche Dokumentation zeigt bewusst nur Architektur, Ablauf und Ergebnis.  
Detailnotizen, interne Pfade, vollständige Konfigurationen, YAML-Dateien, Runner-Details und Troubleshooting bleiben lokal.

---

## Geplante Erweiterungen

- Ausbau der Pipeline in klarere Build-, Test- und Deployment-Stufen
- Erweiterung der automatisierten Tests
- Ergänzung ausgewählter, bereinigter Screenshots
- Health-Check für die bereitgestellte Anwendung
- Vergleich zwischen Self-hosted Runner und gehosteten GitHub Actions Runnern
- Ausbau des Kubernetes-Deployments
- optionale Monitoring-Erweiterung
- laufende Aktualisierung der README als kompakte Portfolio-Dokumentation

---

## Kurzfazit

Das Mini CI/CD Lab zeigt einen vollständigen technischen Delivery-Prozess von der lokalen Codeänderung bis zum Kubernetes Deployment.  
Der Fokus liegt auf sauberer Automatisierung, nachvollziehbarer Struktur, kontrollierter Dokumentation und praxisnaher Umsetzung.
