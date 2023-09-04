# Webarbeitsplatz

Der Webarbeitsplatz ist eine integrierte Plattform, die verschiedene kollaborative Anwendungen wie E-Mail, Chat, Dateiablage, Videokonferenzen, Monitoring und Single Sign-On bereitstellt. Dieses Projekt verwendet Docker, Kubernetes (Minikube) und den Service Mesh Istio, um die genannten Anwendungen zu verwalten und zu betreiben.

## Komponenten

- **iRedMail**: Eine E-Mail-Server-Lösung.
- **Zulip**: Eine Chat-Plattform für Teams.
- **Nextcloud**: Eine Plattform für Dateispeicherung und Zusammenarbeit.
- **Jitsi**: Eine Plattform für Videokonferenzen.
- **Grafana**: Eine Open-Source-Monitoring-Lösung.
- **Keycloak**: Eine Open-Source-Identity-and-Access-Management-Lösung.
- **Istio**: Ein Service Mesh zur Verwaltung von Diensten und Kommunikation.

## Lösungskonzept

## IT-Grundschutz-Baustein 

### Aktive
+ APP.3.1 Webanwendungen 
+ NET.1.2 Netzmanagement 
+ ORG 2.6 Mobiles Arbeiten und Telearbeit 

### Passiv:
+  SYS.1.1 Allgemeiner Server 
+  SYS.1.3 Server unter Linux und Unix
+  APP.3.2 Webserver
+ APP 2.4 Relationale Datenbanken
+  APP.4.4 Kubernetes
+ SYS.1.6 Containerisierung
+ APP.5.3 Allgemeiner E-Mail-Client und -Server
+ CON.10 Entwicklung von Webanwendungen
+ NET.3 Netzkomponenten 

##  API-Sicherheit:
   -   **Zero Trust Modell**: Kein Dienst, sei es intern oder extern, hat standardmäßig Zugriff auf andere Dienste. Jeder Dienst muss sich zuerst authentifizieren und wird erst dann autorisiert.
   -   **TLS-Enforced**: Der gesamte Datenverkehr zwischen den Services und zum API-Gateway wird über (m)TLS verschlüsselt, und die API ist anders nicht erreichbar. Das stellt sicher, dass die Daten sowohl in Ruhe als auch in Bewegung sicher sind.
   -   **Dienst-zu-Dienst-Authentifizierung**: Vor Nutzung der API oder anderen Diensten müssen sich alle Dienste authentifizieren.


## Technologieauswahl
Technologieauswahl basiert auf [landscape.cncf](https://landscape.cncf.io/) und den [IT-Grundschutz-Baustein](https://www.bsi.bund.de/DE/Themen/Unternehmen-und-Organisationen/Standards-und-Zertifizierung/IT-Grundschutz/it-grundschutz_node.html)

### 1. API-Gateway: Ambassador

Ein Open-Source-API-Gateway, LDAP-kompatibel. Es agiert als Hauptzugangspunkt, leitet Anfragen weiter und stellt sicher, dass nur berechtigte durchgelassen werden.

-   **Berücksichtigung von IT-Grundschutz-Bausteinen**:
    -   APP.3.1 Webanwendungen: Authentifizierung, Autorisierung, Rate-Limiting durch Ambassador.
    -   ORG 2.6 Mobiles Arbeiten und Telearbeit: Sichere Zugriffsverwaltung auf Services auch von Remote-Standorten durch Integration mit Istio.

### 2. Service Mesh: Istio

Open-Source-Service-Mesh für Netzwerkverkehr, Sicherheit und Observability zwischen den Services.

-   **Berücksichtigung von IT-Grundschutz-Bausteinen**:
    -   NET.1.2 Netzmanagement: Netzwerksicherheit für Microservices durch Istio.
    -   APP.3.1 Webanwendungen: Authentifizierung, Autorisierung, und (m)TLS durch Istio.

### 3. Authentifizierung: OAuth2 mit LDAP-Integration: Keycloak

Kombiniert moderne Token-basierte Authentifizierung mit traditionellem LDAP-Verzeichniszugriff..

-   **Berücksichtigung von IT-Grundschutz-Bausteinen**:
    -   APP.3.1 Webanwendungen: Sichere Kommunikation und feingranulare Zugriffskontrolle.
    -   ORG 2.6 Mobiles Arbeiten und Telearbeit: Adaptive Authentifizierung und Single Sign-On (SSO) Unterstützung.

## Umsetzung:

-  **API-Gateway-Einrichtung:** Verwaltung von eingehenden Anfragen und Weiterleitung an entsprechende Dienste.
  -   **Service Mesh:** Steuert die Mikrodienst-Kommunikation, ermöglicht Überwachung und fügt eine Sicherheits- und Observabilitätsschicht hinzu.
-   **Authentifizierung mit Keycloak:** Keycloak wird als OAuth2-Server eingesetzt und mit LDAP integriert, um als zentrale Benutzerauthentifizierungs- und Autorisierungsquelle zu dienen und Tokens zu generieren.
    
**Sicherheit und IT-Grundschutz-Bausteine:**

1.  **Datenverkehrssicherheit mit Istio:** Verkehr wird über (m)TLS verschlüsselt und ermöglicht Zugriffskontrollrichtlinien.
2.  **Infrastrukturschutz:** Absicherung durch Istio, das Netzwerk-Isolation und Verschlüsselung sicherstellt.
3.  **Virtualisierung und Webservice-Sicherheit:** Gewährleistet durch den isolierten und kontrollierten Service Mesh sowie das API-Gateway.
4.  **API-Sicherheit:** OAuth2 mit Keycloak bietet Token-basierte Authentifizierung und Autorisierung.
5.  **Mobiles Arbeiten und Telearbeit:** Gesicherter Zugriff über Istio und OAuth2 wird gewährleistet.
    

**Interoperabilität und Modularität:**

1.  **Service Modularität:** Einfaches Hinzufügen oder Entfernen von Services im Service Mesh.
2.  **Vermeidung von Vendor-Lock-In:** Nutzung von offenen Standards und Open-Source-Tools.
3.  **API-Gateway:** Schnittstelle zur Integration von Drittanbieter-Apps und Services.

**Authentifizierung und Autorisierung:**

1.  **Dashboard Authentifizierung:** Die Dashboard-Anwendung authentifiziert sich über Keycloak, das OAuth2 für die Integration mit dem LDAP-Server verwendet.
2.  **Token-Authentifizierung:** Token werden für Benutzer generiert und zur Authentifizierung von Service-zu-Service-Anfragen im Netzwerk verwendet.

**Erweiterbarkeit:**

1.  **Interoperabilitäts-Use-Cases:** Das System kann durch die Hinzufügung von Mikroservices oder Integrationen über das API-Gateway erweitert werden.
## Anforderungen

- [Docker](https://www.docker.com/)
- [Minikube](https://minikube.sigs.k8s.io/)

## Installation

1. Stellen Sie sicher, dass Docker und Minikube auf Ihrem System installiert sind.
2. Klonen Sie dieses Git-Repository:

   ```sh
   git clone https://github.com/user2595/Webarbeitsplatz)https://github.com/user2595/Webarbeitsplatz
   cd Webarbeitsplatz
