Thema: Abfahrten in der Nähe
Idee: Ein System, das im Hamburger Nahverkehr anzeigt, welche Busse oder Bahnen in der Nähe abfahren.
externer  API (HVV Open Data),
Backend-Logik, die Daten verarbeitet,
Potenzial für Verteilung auf mehrere Rechner/Services
evtl. einer Frontend-Komponente (z. B. ein Smart Display oder eine Webansicht)
1. API-Anbindung an externen Dienst (GraphQL oder RESTful)
Machbar mit HVV API
Der Hamburger Verkehrsverbund (HVV) bietet eine REST-API über das HVV Open Data Portal oder Geofox API.
Darüber kannst du Echtzeit-Abfahrten für eine Station oder geografische Koordinaten abfragen.
2. Interne Kommunikation (ICC) über RPC
Service
Aufgabe
Kommunikation
A: Location Service
Nimmt Koordinaten entgegen und sucht nächstgelegene Haltestellen
gRPC-Call an Service B
B: Departure Service
Fragt über die HVV API die nächsten Abfahrten ab
Antwort an A über gRPC
C: Frontend/Display Service(optional)
Zeigt Daten auf einem Gerät oder Web-Frontend an
Fragt A über HTTP/gRPC an

Damit nutzt du gRPC oder RMI intern → erfüllt das „Intern RPC“ Kriterium.

3. Loadsharing
mehrere Instanzen des Departure Service starten (z. B. in Docker),
und sie über einen Load Balancer (z. B. NGINX, gRPC Load Balancing, oder einfache Round-Robin-Logik) verteilen.
Skalierbarkeit vom System und Loadsharing nachweisen.

4. Service Orchestrierung über RPC
Ein Service (z. B. „Coordinator“) ruft mehrere andere RPC-Services auf, um eine zusammengesetzte Antwort zu bilden.
Beispiel:
Der Orchestrator-Service bekommt vom Frontend den Standort.
Er ruft:
LocationService → findet Haltestellen,
DepartureService → holt Abfahrten,
evtl. AIService → berechnet Prognose (z. B. „Wie wahrscheinlich ist eine Verspätung?“),
aggregiert die Ergebnisse und gibt sie zurück.

5. Optionale AI-Komponente:
Eine kleine Machine-Learning-Komponente, die aus historischen Daten oder Live-Feeds Verspätungen vorhersagt.
Oder: Ein Recommendation-Service, der die beste Verbindung vorschlägt, abhängig von der Uhrzeit, Wetter o. ä.
oder einfach gehalten:
Ein „Pseudo-AI-Service“, der Regeln (Heuristiken) anwendet, z. B. „Bei Regen + Stoßzeit = +5 Minuten Verspätung“.
