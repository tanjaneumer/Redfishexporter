# Redfishexporter
## Image im dockerhub
    docker run -d -p 9393:9393 -e PORT=9393 -e HOST_IP=0.0.0.0 -e VERSION_AS_INTEGER=1 -e USER=redfishuser -e PASSWORD=redfishpw -e PERIOD=60 tneumertubredfish_exporter_env

* environment:
  * HOST_IP: IP des HW-Servers
  * VERSION_AS_INTEGER: Redfish API Version (momentan gibt es nur Version 1, daher default 1)
  * USER: Angelgter User auf dem HW-Server
  * PASSWORD: Passwort des Users auf dem HW-Server
  * PERIOD: Periode in Sekunden für die HW-Server Abfrage (default 60)
  * PORT: Port, auf dem die Metriken zu finden sind (default 9393)
  
## Was ist Redfish
Redfish Scalable Platforms Management API eine auf REST basierte Softwareschnittstelle, die die Fernwartung von Servern erlaubt. Das Spannende ist, dass es sich um 
ein sogenanntes Out-of-band System handelt, ein system das unabhängig vom eigentlichen Server läuft. Dies bringt im Monitoringbereich Vorteile zum herkömmlichen 
Abfragen der Hardware wie beispielsweise durch SNMP. Zum einen kann der Zustand der Hardware selbst bei einem ausgeschaltetem Server überprüft werden, 
außerdem ist kein Betriebssystem und keine Treiber auf dem Server nötig um Abfragen durchzuführen.

Ein Anwendungsbeispiel für das Monitoring wäre, dass der Server herunterfährt, weil er zu heiß geworden ist. Das heißt lokal wäre nicht nachvollziehbar was passiert ist, 
da der Server aus ist. Hier kommt das Out-of-Band Prinzip ins Spiel, das es erlaubt die Hardwarezustände weiterhin zu monitoren und zu ermitteln warum das Gerät aus ist.

Im speziellen Fall von HP Servern sieht das HP-Betriebssystem die physischen Festplatten nicht, da über Controller darauf zugegriffen wird. Das heißt, fällt eine Festplatte 
aus, wird dies durch den Controller ausgeglichen und das Betriebssystem erfährt davon nichts. Damit sind Abfragen über Softwareschnittstellen für diese Art Problem
nicht geeignet. Dafür müsste ein Out-of-Bound System genutzt werden, was mittels Redfish ansteuerbar ist.

Im Großen und Ganzen soll Redfish IPMI ablösen und übernimmt und erweitert dessen Aufgaben. Hintergründe und weitere Informationen sind unter httpredfish.dmtf.org und httpswww.dmtf.orgsitesdefaultfilesstandardsdocumentsDSP2044_1.0.0.pdf  zu finden.

### Unterschied von SNMP und Redfish
Sowohl SNMP, als auch Redfish sind Protokolle um Maschineninformationen abzufragen. Dabei wird Redfish vorwiegend bei Servern verwendet, da es zusätzlich die Steuerung 
des Servers, auch in ausgeschaltetem Zustand, ermöglicht. 

Der große Unterschied der beiden liegt darin, dass Redfish eine Spezifikation für Out-of-Band Geräte ist und darüber Maschineninformationen liefert, unabhängig der 
Server-Plattform und Hardware-Komponenten. Dem gegenüber steht SNMP, was eine Software-Spezifikation ist, die meist in-bound genutzt wird. Es werden Maschineninformationen 
anhand des Betriebssystem, auf dem es läuft, geliefert.

## Designidee
Der Exporter besteht aus 
 einem Modul, dass die Redfish API abfrägt und die json-Antworten übergibt
 einem Modul, dass die json-Antowrt übersetzt in Prometheus Metriken und übergibt
     dabei sind die keys oder values beginnend mit @ zu beachten. Dabei handelt es sich um weitere Redfish URLs, die abfragbar sind
     überlegen, wie die Werte gespeichert werden sollen
 einem Modul, dass die MEtriken für Prometheus veröffentlicht
 
 

