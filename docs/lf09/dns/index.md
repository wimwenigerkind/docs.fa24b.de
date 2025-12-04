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

# DNS-Server-Typen und ihre Aufgaben

# Der DNS-Auflösungsprozess

# DNS-Record-Typen (Resource Records)

# DNS-Caching und TTL (Time to Live)

# DNS-Sicherheit: Bedrohungen und Schutzmaßnahmen

# Praktische DNS-Tools und Befehle

# DNS in der Praxis: Konfiguration und Verwaltung

# Zusammenfassung und Ausblick