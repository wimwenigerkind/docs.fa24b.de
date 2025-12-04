# DNS

# Was ist DNS und warum brauchen wir es?

Das **Domain Name System (DNS)** funktioniert wie ein Telefonbuch für Netzwerke. Seine Hauptfunktion ist die Umwandlung
von menschenlesbaren Domainnamen (wie [docs.fa24b.de](https://docs.fa24b.de)) in maschinenlesbare
IP-Adressen (wie 185.199.108.153). So können Webbrowser, E-Mail-Clients und andere Netzwerkdienste die
richtigen Server erreichen – sowohl im Internet als auch in lokalen Netzwerken.

## Warum ist DNS notwendig?

Computer kommunizieren über IP-Adressen miteinander. Für Menschen sind diese Zahlenfolgen jedoch schwer zu merken und unpraktisch.
Stellen Sie sich vor, Sie müssten sich für jede Website die entsprechende IP-Adresse merken:

- **Mit DNS**: Sie geben `www.google.com` ein
- **Ohne DNS**: Sie müssten `142.250.185.46` eingeben

DNS löst dieses Problem, indem es als Vermittler zwischen Menschen und Maschinen fungiert.

## Die Hauptaufgaben des DNS

1. **Namensauflösung**: Übersetzung von Domainnamen in IP-Adressen (Forward Lookup)
2. **Reverse Lookup**: Übersetzung von IP-Adressen zurück in Domainnamen
3. **Lastverteilung**: Verteilung von Anfragen auf mehrere Server
4. **Redundanz**: Bereitstellung von Ausfallsicherheit durch mehrere DNS-Server
5. **E-Mail-Routing**: Bestimmung zuständiger Mailserver über MX-Records

## Beispiel einer DNS-Anfrage

Wenn Sie `docs.fa24b.de` in Ihren Browser eingeben, passiert Folgendes:

1. Der Browser fragt den lokalen DNS-Resolver nach der IP-Adresse
2. Falls nicht im Cache vorhanden, werden verschiedene DNS-Server abgefragt
3. Die IP-Adresse `185.199.108.153` wird zurückgeliefert
4. Der Browser kann nun eine Verbindung zum Webserver herstellen

Dieser gesamte Prozess dauert nur wenige Millisekunden und läuft vollautomatisch im Hintergrund ab.

# Aufbau und Hierarchie des DNS

Das \**Domain Name System (DNS)\** ist hierarchisch aufgebaut und weltweit verteilt. Diese Struktur ermöglicht eine
skalierbare, ausfallsichere und dezentrale Verwaltung von Domainnamen.

## Aufbau einer Domain

Eine Domain wie `www.example.com` wird von rechts nach links gelesen:

- `.` \- die (meist unsichtbare) \**Root\-Zone\**
- `com` \- \**Top\-Level\-Domain (TLD)\**
- `example` \- \**Second\-Level\-Domain\**
- `www` \- \**Subdomain\** bzw. Hostname

Jede Ebene kann von unterschiedlichen Organisationen verwaltet werden.

## Root\-Ebene

An der Spitze der Hierarchie steht die \**Root\-Zone\**:

- wird von einer kleinen Anzahl von \**Root\-Servern\** betrieben
- kennt keine einzelnen Hostnamen
- verweist nur auf die zuständigen Nameserver der TLDs (z\.B. `.de`, `.com`, `.org`)

Ohne die Root\-Server könnte das DNS nicht von Grund auf funktionieren.

## Top\-Level\-Domains (TLDs)

Unterhalb der Root\-Zone befinden sich die \**Top\-Level\-Domains\**:

- \**Generische TLDs (gTLDs)\**: z\.B. `.com`, `.org`, `.net`
- \**Länderspezifische TLDs (ccTLDs)\**: z\.B. `.de`, `.fr`, `.ch`

Die TLD\-Server wissen, welche Nameserver für die darunterliegenden Domains zuständig sind, z\.B.:

- `example.com` → autoritative Nameserver von `example.com`
- `schule.de` → autoritative Nameserver von `schule.de`

## Second\-Level\-Domains

Die \**Second\-Level\-Domain\** ist der Teil direkt unterhalb der TLD:

- `example` in `example.com`
- `schule` in `schule.de`

In diesem Bereich verwalten typischerweise Firmen, Organisationen oder Provider ihren eigenen DNS\-Namensraum. Dort
werden u\.a. festgelegt:

- welche Hostnamen existieren (z\.B. `www.example.com`, `mail.example.com`)
- auf welche IP\-Adressen diese zeigen
- welche Mailserver zuständig sind (MX\-Records)
- welche Nameserver autoritativ für die Zone sind (NS\-Records)

## Subdomains und Delegation

Unterhalb der Second\-Level\-Domain können beliebig viele \**Subdomains\** angelegt werden:

- `www.example.com` \- Webserver
- `mail.example.com` \- Mailserver
- `intranet.example.com` \- internes Portal 

Subdomains können wiederum zu \**eigenen Zonen\** mit eigenen autoritativen Nameservern werden. Dies nennt man
\**Delegation\**. So lässt sich die Verwaltung auf verschiedene Abteilungen oder Standorte aufteilen.

## Zonen und Verantwortlichkeiten

Wichtiger Begriff im DNS ist die \**Zone\**:

- eine Zone ist der Teil der DNS\-Hierarchie, der von einem Satz autoritativer Nameserver verwaltet wird
- eine Domain kann mehrere Zonen enthalten, wenn Teilbereiche delegiert werden

Beispiel:

- `schule.de` wird zentral vom Schulträger verwaltet
- `it.schule.de` wird an die IT\-Abteilung delegiert und von deren eigenen Nameservern verwaltet

Dadurch entsteht eine \**dezentrale\**, aber logisch klar strukturierte Verwaltung des Namensraums.


# DNS-Server-Typen und ihre Aufgaben

# Der DNS-Auflösungsprozess

# DNS-Record-Typen (Resource Records)

# DNS-Caching und TTL (Time to Live)

# DNS-Sicherheit: Bedrohungen und Schutzmaßnahmen

# Praktische DNS-Tools und Befehle

# DNS in der Praxis: Konfiguration und Verwaltung

# Zusammenfassung und Ausblick