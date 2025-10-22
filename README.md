from pathlib import Path

# Thema: Abfahrten in der Nähe

## 💡 Idee  
Ein System, das im **Hamburger Nahverkehr** anzeigt, welche **Busse oder Bahnen** in der Nähe abfahren.

### Komponenten:
- Externe **API-Anbindung** (HVV Open Data)  
- **Backend-Logik**, die Daten verarbeitet  
- Potenzial für **Verteilung auf mehrere Rechner/Services**  
- Optionale **Frontend-Komponente** (z. B. Smart Display oder Webansicht)

---

## ⚙️ 1. API-Anbindung an externen Dienst (GraphQL oder RESTful)

Machbar mit der **HVV API**.  
Der **Hamburger Verkehrsverbund (HVV)** bietet eine **REST-API** über das  
[HVV Open Data Portal](https://www.hvv.de/de/fahrplaene/open-data) oder die **Geofox API**.

Über diese Schnittstelle können **Echtzeit-Abfahrten** für eine Station oder geografische Koordinaten abgefragt werden.

---

## 🔗 2. Interne Kommunikation (ICC) über RPC

### Übersicht der Services

| Service | Aufgabe | Kommunikation |
|----------|----------|----------------|
| **A: Location Service** | Nimmt Koordinaten entgegen und sucht nächstgelegene Haltestellen | gRPC-Call an Service B |
| **B: Departure Service** | Fragt über die HVV API die nächsten Abfahrten ab | Antwort an A über gRPC |
| **C: Frontend/Display Service (optional)** | Zeigt Daten auf einem Gerät oder im Web-Frontend an | Fragt A über HTTP/gRPC an |

➡️ Damit nutzt das System **gRPC oder RMI intern** und erfüllt das Kriterium *„Internal RPC“*.

---

## ⚖️ 3. Loadsharing

- Mehrere **Instanzen des Departure Service** starten (z. B. mit **Docker**).  
- Verteilung über einen **Load Balancer**, z. B.:
  - **NGINX**
  - **gRPC Load Balancing**
  - oder eine einfache **Round-Robin-Logik**

📈 Ziel: **Skalierbarkeit** des Systems und **Loadsharing** nachweisen.

---

## 🧩 4. Service-Orchestrierung über RPC

Ein spezieller **Coordinator/Orchestrator-Service** ruft mehrere andere RPC-Services auf, um eine **zusammengesetzte Antwort** zu bilden.

### Beispiel:
1. Das Frontend sendet den Standort an den Orchestrator.  
2. Der Orchestrator ruft folgende Services auf:
   - **LocationService** → findet Haltestellen  
   - **DepartureService** → holt Abfahrten  
   - **AIService (optional)** → berechnet Prognosen (z. B. Verspätungswahrscheinlichkeit)  
3. Die Ergebnisse werden aggregiert und als **einheitliche Antwort** zurückgegeben.

---

## 🤖 5. Optionale AI-Komponente

Eine kleine **Machine-Learning-Komponente**, die z. B. aus historischen Daten oder Live-Feeds **Verspätungen vorhersagt**.

### Alternativ:
Ein **Recommendation-Service**, der:
- die **beste Verbindung** vorschlägt (z. B. abhängig von Uhrzeit, Wetter, Auslastung),
- oder einfach als **Pseudo-AI-Service** Regeln anwendet, etwa:

```text
Wenn Regen + Stoßzeit → +5 Minuten Verspätung
