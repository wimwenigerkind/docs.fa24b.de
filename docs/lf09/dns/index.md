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

Das Domain Name System ist hierarchisch aufgebaut. An der Spitze stehen die Root-Server, gefolgt von Top-Level-Domains (TLDs), autoritativen Nameservern für Zonen und rekursiven Resolvern, die Anfragen im Auftrag von Clients bearbeiten. Die Hierarchie ermöglicht eine skalierbare und verteilte Verwaltung von Namensräumen.

## Root-Zone und TLDs

Die Root-Zone enthält Delegationen zu Top-Level-Domains. TLD-Nameserver verwalten Einträge für Second-Level-Domains oder delegieren weiter an autoritative Nameserver.

## Autoritative Nameserver und Rekursive Resolver

Autoritative Nameserver verwalten die endgültigen DNS-Zonen und beantworten Anfragen zu ihren Zonen mit signierten bzw. autoritativen Antworten. Rekursive Resolver führen die eigentliche Auflösung durch, indem sie die Hierarchie abfragen und Antworten zwischen Caches und Clients vermitteln.

# DNS-Server-Typen und ihre Aufgaben

Dieser Abschnitt beschreibt die in der Praxis üblichen DNS-Server-Typen und deren typische Funktionen.

## Autoritative Nameserver

- **Aufgabe**: Liefern endgültige Antworten für die verwalteten Zonen.
- **Einsatz**: Werden für Domains betrieben, um Zone-Records wie A, AAAA, MX, NS, SOA bereitzustellen.
- **Betriebshinweis**: Redundanz wird durch mehrere autoritative Server an verschiedenen Standorten (Anycast oder mehrere IP-Adressen) erreicht.

## Rekursive Resolver

- **Aufgabe**: Beantworten Client-Anfragen, indem sie selbstständig die notwendigen Abfragen an andere Nameserver durchführen.
- **Einsatz**: Typischerweise in Betriebssystemen, LAN-Routern, ISPs oder Public DNS-Diensten vorhanden.
- **Sicherheitsaspekt**: Offene rekursive Resolver sollten vermieden werden, da sie für DNS-Amplification missbraucht werden können.

## Caching-Server und Forwarder

- **Aufgabe**: Speichern kürzlich abgefragte Einträge zur Leistungsverbesserung und reduzieren wiederholte Netzwerklast.
- **Einsatz**: In lokalen Netzwerken oder als Zwischeninstanz zu einem Upstream-Resolver.

# Der DNS-Auflösungsprozess

Der Auflösungsprozess erläutert die Schritte, die ein rekursiver Resolver oder ein Client bei der Übersetzung eines Domainnamens in eine IP-Adresse durchführt.

## Schritte der rekursiven Auflösung

1. **Lokaler Cache-Check**: Zuerst wird der lokale Cache auf vorhandene Einträge geprüft.
2. **Root-Server-Abfrage**: Falls notwendig, wird die Anfrage an einen Root-Server weitergeleitet, um die zuständige TLD-Delegation zu erhalten.
3. **TLD-Abfrage**: Abfrage der TLD-Nameserver zur Ermittlung des autoritativen Servers für die Zielzone.
4. **Autoritative Abfrage**: Der autoritative Nameserver der Zone wird kontaktiert und liefert die endgültige Antwort.
5. **Antwort und Caching**: Die erhaltene Antwort wird an den anfragenden Client zurückgegeben und gemäß TTL gecached.

## Iterative vs. rekursive Abfragen

- **Rekursive Abfragen**: Ein Resolver übernimmt die gesamte Folge von Abfragen bis zur finalen Antwort.
- **Iterative Abfragen**: Nameserver antworten mit Verweis auf die nächste Stufe (z. B. TLD-Server), und der anfragende Resolver führt die nächsten Schritte aus.

## Fehlerszenarien und Diagnostik

- **NXDOMAIN**: Die Domain ist nicht vorhanden; häufig durch einen NXDOMAIN-Status im SOA begründet.
- **SERVFAIL**: Probleme beim autoritativen Server oder bei DNSSEC-Validierung.
- **Timeouts**: Netzwerk- oder Serververfügbarkeitsprobleme.

# DNS-Record-Typen (Resource Records)

DNS-Records beschreiben verschiedene Eigenschaften einer Domain und steuern Dienste wie Web, E-Mail und Authentifizierung. Die wichtigsten Typen sind im Folgenden zusammengefasst.

## Wichtige Record-Typen

- **A / AAAA**: Adress-Records für IPv4 bzw. IPv6, die Hostnamen IP-Adressen zuordnen.
- **CNAME**: Alias-Record, der einen Namen auf einen anderen Namen verweist; CNAMEs dürfen nicht zusammen mit anderen Record-Typen für denselben Namen existieren.
- **MX**: Mail-Exchange-Records zur Bestimmung zuständiger Mailserver einer Domain, inklusive Priorität.
- **NS**: Nameserver-Records, die die autoritativen Server für eine Zone delegieren.
- **SOA**: Start of Authority; enthält Metadaten zur Zone (z. B. Seriennummer, Refresh-Intervalle, Retry, Expire, Minimum TTL).
- **TXT**: Freitextfelder, oft für SPF, DKIM, DMARC oder andere verifizierende Informationen verwendet.
- **SRV**: Service-Records zur Angabe von Diensten mit Port und Priorität (z. B. für SIP, XMPP).

## Anwendungs- und Konfigurationshinweise

- **Reihenfolge und Priorität**: MX-Records nutzen Prioritäten; mehrere A/AAAA-Records können zur Lastverteilung eingesetzt werden.
- **Vermeidung von Fehlkonfigurationen**: CNAME-Einschränkungen beachten und SOA-Seriennummer bei Zonenänderungen inkrementieren.

# DNS-Caching und TTL (Time to Live)

Caching reduziert Latenz und Serverlast, beeinflusst jedoch die Zeitspanne, bis Änderungen an DNS-Records global sichtbar werden.

## Funktionsweise des Caches

- **TTL-Feld**: Jeder Record enthält eine TTL-Angabe, die bestimmt, wie lange ein Resolver den Record zwischenspeichert.
- **Cache-Hierarchie**: Caches existieren auf mehreren Ebenen (Client/Browser, Betriebssystem, lokaler Resolver, ISP) und wirken kumulativ auf die Sichtbarkeit von Änderungen.

## Strategien für TTL-Wahl

- **Kürzere TTLs**: Erlauben schnelle Rollbacks und Änderungen, erhöhen jedoch die Anzahl direkter Abfragen an autoritative Server.
- **Längere TTLs**: Reduzieren Last und verbessern Performance, verzögern jedoch die Propagation von Änderungen.

## Negative Caching und SOA

- **Negative Caching**: Fehlende Einträge (z. B. NXDOMAIN) können für eine durch SOA definierte Zeit zwischengespeichert werden.
- **SOA-Parameter**: Die SOA-Record-Felder steuern u. a. Refresh- und Retry-Verhalten sowie das negative Caching.

# DNS-Sicherheit: Bedrohungen und Schutzmaßnahmen

DNS ist ein häufiges Ziel von Manipulation und Missbrauch; entsprechende Gegenmaßnahmen verbessern Integrität und Verfügbarkeit.

## Häufige Bedrohungen

- **Cache Poisoning / Spoofing**: Einschleusen falscher Antworten in Caches.
- **DDoS und Amplification**: Ausnutzung offener Resolver zur Verstärkung von Angriffen.
- **Man-in-the-Middle und Hijacking**: Umleitung von Traffic durch kompromittierte Resolver oder Manipulation von Zonendaten.

## Schutzmaßnahmen

- **DNSSEC**: Signiert DNS-Antworten und ermöglicht die Validierung der Authentizität und Integrität der Daten.
- **Absicherung von Resolvern**: Keine offenen rekursiven Resolver betreiben; Einsatz von Zugriffskontrollen und Ratenbegrenzung.
- **Netzwerkmaßnahmen**: Einsatz von Anycast, Firewall-Regeln, Traffic-Filtering und DDoS-Schutzdiensten.
- **Betriebliche Maßnahmen**: Regelmäßige Überprüfung von Zone-Transfers, Logging und Monitoring, sowie kontrollierte Schlüsselverwaltung für DNSSEC.

# Praktische DNS-Tools und Befehle

Für Diagnose, Monitoring und Konfiguration stehen mehrere etablierte Werkzeuge zur Verfügung. Beispiele und typische Verwendungsfälle sind nachfolgend dokumentiert.

## Abfrage- und Debugging-Tools

- **dig**: Umfangreiches Tool für detaillierte Abfragen und Debugging (`dig +nocmd +noall +answer example.com ANY`).
- **nslookup**: Einfacher interaktiver Client für grundlegende Abfragen.
- **host**: Kompaktes Werkzeug für schnelle Lookups und Reverse-Lookups.

## Netzwerkanalyse und Systemwerkzeuge

- **tcpdump / Wireshark**: Mitschnitt und Analyse von DNS-Paketen zur Fehlersuche und Sicherheitsanalyse.
- **resolvectl / systemd-resolve**: Verwaltung und Abfrage des systemweiten Resolvers auf systemd-Systemen.

## Einsatzempfehlungen

- **Skriptgestützte Prüfungen**: Automatisierte Checks (z. B. Zonenkonsistenz, SOA-Seriennummern) für regelmäßige Überwachung.
- **Monitoring-Integration**: DNS-Checks in das Monitoring aufnehmen (Latenz, Fehlerquote, DNSSEC-Validierungsstatus).

# DNS in der Praxis: Konfiguration und Verwaltung

Konfigurations- und Betriebsaspekte umfassen Zonendateiverwaltung, Deploy-Prozesse, Monitoring und Backup-Strategien.

## Zonendateiverwaltung

- **Versionsverwaltung**: Änderungen an Zonendateien sollten versioniert und signiert (bei DNSSEC) werden.
- **Deploy-Prozesse**: Stufenweise Rollouts mit abgestimmten TTLs minimieren Unterbrechungen.

## Betrieb und Monitoring

- **Proaktive Überwachung**: Verfügbarkeit autoritativer Server, Antwortzeiten, Fehlerquoten und DNSSEC-Status überwachen.
- **Notfallkonzepte**: Fallback-Nameserver und Disaster-Recovery-Prozeduren vorhalten.

# Zusammenfassung und Ausblick

Die vorgestellten Konzepte bilden die Grundlage für ein zuverlässiges DNS-Design: klare Trennung von Rollen (autoritative Server vs. Resolver), sinnvolle TTL-Strategien, Absicherung gegen verbreitete Angriffe und Einsatz von Werkzeugen zur Überwachung und Fehleranalyse. Zukünftige Entwicklungen, insbesondere in den Bereichen DNS-over-HTTPS/-TLS und verbesserte DNSSEC-Operationalisierung, verändern die Implementierungsdetails und sollten bei weiteren Planungen berücksichtigt werden.