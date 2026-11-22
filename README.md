# Schritt-für-Schritt-Einführung in das React-Framework

📖 **Das Tutorial lesen: [https://stahe.github.io/de-react-etape-par-etape-sept-2026/](https://stahe.github.io/de-react-etape-par-etape-sept-2026/)**

Dieser Kurs zeigt, wie man mit der Bibliothek [React](https://react.dev) 19.3 eine Webanwendung erstellt: eine **Single-Page-Anwendung** (SPA), bei der die Seiten **im Browser** aus JSON-Daten eines Servers gerendert werden.

Er knüpft an den Kurs [Schritt-für-Schritt-Einführung in das Web-Framework NestJS](https://stahe.github.io/de-nestjs-html-sept-2026/) an, bietet eine andere Sichtweise und folgt dem Aufbau der Kurse [Schritt-für-Schritt-Einführung in das Framework Vue.js](https://stahe.github.io/de-vuejs-etape-par-etape-sept-2026/) und [Schritt-für-Schritt-Einführung in das Framework Angular](https://stahe.github.io/de-angular-etape-par-etape-sept-2026/): derselbe Server, dieselben Seiten, im React-Stil geschrieben.

| NestJS-Kurs | React-Kurs |
|---|---|
| der Server erzeugt HTML-Seiten (Handlebars) | der Browser erzeugt die Seiten (React) |
| der Browser zeigt an, was er empfängt | der Server liefert nur JSON |
| Controller, Views, `res.render` | Komponenten, Router, Hooks |
| Guards `JwtAuthGuard`, `RolesGuard` | Router-Guards (React-Router-Middleware); der Server führt seine eigenen |
| vom Server gelesene Wörterbücher | Wörterbücher auf dem Client (i18next); der Server liefert nur Schlüssel |
| Flash-Meldung in einem Cookie | Flash-Meldung in einem Zustandsspeicher (State Store) |

Die Seiten selbst bleiben unverändert: Es sind die der Anwendung **RdvMedecins**, die bereits mit den anderen Frameworks vorgestellt wurde.

## Der Ansatz: viele kurze Beispiele, dann eine Fallstudie

Der Kurs ist um **25 kurze Beispiele** aufgebaut, die sich jeweils auf ein einziges Konzept konzentrieren. Sie bilden ein einziges Vite-Projekt: ein einziger Befehl `npm install`, dann `npm start <Beispiel>`, um eines auszuführen.

| Kapitel | Inhalt | Beispiele |
|---|---|---|
| Einstieg | ein Vite-Projekt, Funktionskomponenten, JSX, Zustand (`useState`, `useReducer`), Ereignisse, kontrollierte Felder, Validierung, React Hook Form, Formatierung (`Intl`) | 01–09 |
| Komponenten | Props, Funktions-Props, `children`, Lebenszyklus (`useEffect`, `useEffectEvent`), Kontexte, eigene Hooks, Bestätigungsdialog (`createPortal`) | 10–16 |
| Routing | React Router 8: Routen, Parameter, Query, Lazy Loading, `<title>`, Middleware | 17–18 |
| Asynchrone Operationen und gemeinsamer Zustand | Timer, Anti-Prellen (Debounce), veraltete Antworten, `use()` + `<Suspense>`, Zustand (Zustand-Bibliothek), `localStorage`, `<Activity>` | 19–20 |
| Internationalisierung | i18next / react-i18next: Parameter, Plurale, Datumsangaben, Beträge | 21 |
| Der Server: eine Blackbox | Einrichtung des JSON-Servers, seine API, 48 `curl`-Beispiele | – |
| Kommunikation mit dem Server | `fetch`, Vite-Proxy, `httpOnly`-Cookies, `useActionState`, API-Zugriffsschicht, TanStack Query, Serverfehler, die Feldern zugeordnet werden | 22–25 |

Jedes Beispiel wird mit seinem vollständigen, zeilenweise kommentierten Code und einem Screenshot seiner Ausführung vorgestellt.

## Der Server: eine Blackbox

Der Server ist der NestJS-Server der vorherigen Lektion, dessen Controller **JSON** zurückgeben – derselbe wie für die Clients in Vue.js und Angular. Diese Lektion behandelt ihn als **Blackbox**: Wir installieren ihn, untersuchen seine API und befragen ihn mit `curl` – seinen Code (der für Neugierige mitgeliefert und kommentiert ist) müssen wir aber nicht lesen.

- Alle Fehler haben dasselbe Format: `{ "statusCode": 409, "cle": "ERRORS.LOGIN_TAKEN", "params": {...}, "champs": {...} }` – **Schlüssel** für die Übersetzung, niemals Klartext;
- Authentifizierung über ein JWT-Token in einem `httpOnly`- / `sameSite=strict`-Cookie: Der clientseitige JavaScript-Code sieht das Token nie;
- CSRF-Schutz: `sameSite`-Cookie, und alle POST-Requests müssen im JSON-Format erfolgen;
- Ein „Testmodus“ für das CAPTCHA, damit die API mit `curl` abgefragt werden kann.

## Die Fallstudie: der React-Client von RdvMedecins

Eine vollständige Anwendung zur **Terminbuchung in einer Arztpraxis**, deren **sämtliche** Dateien (etwa vierzig) aufgelistet und kommentiert werden.

- **Modernes React**: Funktionskomponenten und Hooks, Formular-Actions, `<title>` in Komponenten, React Router 8 im „Data“-Modus (Middleware, bedarfsgesteuertes Laden der Seiten), TanStack Query für Serverdaten, Zustand für gemeinsamen Zustand.
- **Drei Rollen**: `ADMIN` (verwaltet Ärzte und Kunden), `DOCTOR` (plant und storniert Termine), `USER` (der Patient: bucht Termine für sich selbst, verwaltet sein Konto).
- **Datenschutz**: Ein Patient erhält nie die Namen anderer Patienten – der Server sendet sie nicht.
- **Der gesamte Seitenzustand in der URL**: `/agenda?idMedecin=1&jour=2026-10-05&reserver=7` öffnet das Buchungsfenster nach F5 erneut; die Vor-/Zurück-Schaltflächen funktionieren.
- **Serverseitige Validierung**: Formulare zeigen die von der API zurückgegebenen Fehler unter jedem Feld an; optimistisches Sperren, doppelte Namen, bereits vergebener Benutzername usw.
- **Sitzung**: Wird nach F5 wiederhergestellt (`GET /api/auth/moi`), der Ablauf wird an einer einzigen Stelle behandelt (die allen Requests gemeinsame Funktion, Antwort 401).
- **Französisch / Englisch**, übersetzte Tab-Titel, per Tastatur bedienbares Bestätigungsfenster, Bootstrap 5.
- **Bereitstellung**: Der kompilierte Client wird vom JSON-Server selbst ausgeliefert (gleicher Origin, kein CORS).

## Repository-Inhalt

```
exemples-react/            die 25 kurzen Beispiele (ein einziges Vite-Projekt)
rdvmedecins-nestjs-json/   der JSON-Server von RdvMedecins (die „Blackbox“)
rdvmedecins-react/         der React-Client der Fallstudie
```

## Technologien

React 19.3 · TypeScript 6.0 · Vite 8 · React Router 8.4 · TanStack Query 5 · Zustand 5 · i18next 26 / react-i18next 17 · React Hook Form 7 (Beispiel 08) · Bootstrap 5.3 · serverseitig: NestJS 10 · TypeORM · MySQL 8 / MariaDB · Passport JWT · svg-captcha

## Voraussetzungen

- Grundkenntnisse in JavaScript (oder TypeScript), HTML und im HTTP-Protokoll.
- Node.js 24 (oder mindestens 22.22), Visual Studio Code, die Browsererweiterung React Developer Tools und ein MySQL-Server (z. B. Laragon unter Windows) für den JSON-Server. Installationsanleitungen finden sich in den Anhängen des Kurses.

## Autor

Dieser Kurs, seine Beispiele, der JSON-Server und die Fallstudie wurden von **Claude**, der KI von [Anthropic](https://www.anthropic.com) (September 2026), im Auftrag von **Serge Tahé** geschrieben.
