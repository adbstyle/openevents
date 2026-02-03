# Requirements: Open Events

## Vision Statement
**Open Events** ist eine PWA-Plattform, die es Nutzern ermöglicht, Events innerhalb ihrer sozialen Kreise zu teilen und durch überlappende Kreise sowie Following-Mechanismen neue soziale Kontakte zu knüpfen.

**Kernmehrwert:** Die Hemmschwelle senken, an Events teilzunehmen, indem sichtbar wird, welche bekannten Personen bereits teilnehmen.

---

## Kontext & Entscheidungen (aus Stakeholder-Interview)

| Thema | Entscheidung |
|-------|-------------|
| Kreis-Erstellung | Jeder User kann frei Kreise erstellen (max 5) |
| Kreis-Beitritt | Via Einladungslink (max 20 Mitgliedschaften) |
| Kreis-Verwaltung | Admins können: bearbeiten, Mitglieder entfernen, weitere Admins ernennen |
| Kreis-Löschung | Möglich; Events bleiben beim User (verlieren nur Kreis-Zuordnung) |
| Einladungslinks | Mehrfach nutzbar, Admin kann invalidieren/neu erstellen |
| Teilnehmer-Sichtbarkeit | Alle Teilnehmer sichtbar (kreis-übergreifend) |
| Gemeinsame Kreise | Sichtbar auf User-Profil |
| Following | Nur innerhalb eigener Kreise möglich |
| Discovery | Push-Notifications + personalisierter Feed |
| Event-Scope | Nur kreis-basiert (keine öffentlichen Events, keine privaten Events innerhalb eines Kreises) |
| Event-Typen | Einmalig und wiederkehrend |
| Karte | Optional (nice-to-have) |
| Zielgruppe | Freundeskreise + Communities |
| Monetarisierung | Nicht im MVP |
| Teilnahme-Status | Interessiert, Zusage, Vielleicht, Absage |
| Nach-Event Features | Nicht im MVP |

---

## Stakeholder & Personas

### Primäre User-Rollen

| Persona | Beschreibung | Hauptziele |
|---------|-------------|------------|
| **Event-Organisator** | Erstellt und bewirbt Events in seinen Kreisen | Möglichst viele relevante Teilnehmer erreichen |
| **Event-Entdecker** | Sucht nach interessanten Events | Passende Events finden, an denen Bekannte teilnehmen |
| **Kreis-Admin** | Verwaltet einen oder mehrere Kreise | Mitglieder einladen, Kreis aktiv halten |
| **Passiver Nutzer** | Schaut gelegentlich, nimmt selten teil | Informiert bleiben ohne Aufwand |

---

## Epic Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                         EPIC DEPENDENCY MAP                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   [E1: User & Auth] ──────────────────────────────────────────┐     │
│         │                                                      │     │
│         ▼                                                      │     │
│   [E2: Kreise] ─────────────┬──────────────────────┐          │     │
│         │                   │                      │          │     │
│         ▼                   ▼                      ▼          │     │
│   [E3: Events] ──────► [E4: Discovery   ] ◄── [E5: Following] │     │
│         │               & Feed                                │     │
│         ▼                   │                                 │     │
│   [E6: Teilnahme &          ▼                                 │     │
│        Interaktion] ──► [E7: Notifications]                   │     │
│                                                               │     │
│                        [E8: PWA & Offline] ◄──────────────────┘     │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Epic 1: User Management & Authentifizierung

**Ziel:** User können sich registrieren, anmelden und ihr Profil verwalten.

### User Stories

#### US-1.1: Registrierung
**Als** neuer User
**möchte ich** mich mit Email/Passwort oder Social Login registrieren
**damit** ich die Plattform nutzen kann.

**Akzeptanzkriterien:**
1. User kann sich mit Email und Passwort registrieren
2. Email-Verifizierung wird durchgeführt
3. Social Login (Google, Apple) ist möglich
4. User muss Nutzungsbedingungen akzeptieren

#### US-1.2: Login
**Als** registrierter User
**möchte ich** mich anmelden
**damit** ich auf meine Kreise und Events zugreifen kann.

**Akzeptanzkriterien:**
1. Login mit Email/Passwort funktioniert
2. Login mit Social Provider funktioniert
3. "Passwort vergessen" Flow existiert
4. Session bleibt auf dem Gerät erhalten (Remember me)

#### US-1.3: Profil bearbeiten
**Als** User
**möchte ich** mein Profil bearbeiten (Name, Profilbild, Bio)
**damit** andere mich erkennen können.

**Akzeptanzkriterien:**
1. User kann Anzeigename ändern
2. User kann Profilbild hochladen (max 5MB, Bildformate)
3. User kann kurze Bio verfassen
4. Änderungen werden sofort übernommen

---

#### US-1.4: E-Mail-Adresse ändern
**Als** User
**möchte ich** meine E-Mail-Adresse ändern
**damit** ich mein Konto aktuell halten kann.

**Vorbedingungen:**
1. Der USER ist eingeloggt
2. Der USER hat seine E-Mail verifiziert

**Akzeptanzkriterien:**
1. Der USER kann eine neue E-Mail-Adresse eingeben
2. Das SYSTEM sendet einen Verifizierungslink an die neue Adresse
3. Die Änderung wird erst aktiv WENN der USER den Link bestätigt
4. Die alte E-Mail bleibt gültig bis zur Bestätigung
5. Der USER kann den Änderungsvorgang abbrechen

**Nachbedingungen:**
1. Das SYSTEM verwendet die neue E-Mail für Login und Kommunikation

---

#### US-1.5: Passwort ändern
**Als** User
**möchte ich** mein Passwort ändern
**damit** ich mein Konto sicher halten kann.

**Vorbedingungen:**
1. Der USER ist eingeloggt

**Akzeptanzkriterien:**
1. Der USER kann ein neues Passwort setzen (falls E-Mail+Passwort Login)
2. Das SYSTEM validiert Passwort-Stärke (Auth-Provider Standard)
3. Der USER muss das aktuelle Passwort zur Bestätigung eingeben
4. Das SYSTEM bestätigt die Änderung per E-Mail

**Out of Scope:**
1. Passwort-Anforderungen werden vom Auth-Provider definiert

---

#### US-1.6: Passwort zurücksetzen
**Als** User
**möchte ich** mein Passwort zurücksetzen können
**damit** ich bei Vergessen wieder Zugang erhalte.

**Vorbedingungen:**
1. Der USER hat ein Konto mit E-Mail+Passwort Login

**Akzeptanzkriterien:**
1. Der USER kann auf der Login-Seite "Passwort vergessen" wählen
2. Der USER gibt seine E-Mail-Adresse ein
3. Das SYSTEM sendet einen Reset-Link (zeitlich begrenzt gültig)
4. Der USER kann ein neues Passwort setzen nach Klick auf den Link
5. Das SYSTEM meldet Erfolg OHNE zu verraten ob E-Mail existiert (Security)

**Out of Scope:**
1. Reset-Link Gültigkeit wird vom Auth-Provider definiert

---

#### US-1.7: Account löschen (DSGVO)
**Als** User
**möchte ich** meinen Account vollständig löschen können
**damit** meine Daten entfernt werden.

**Vorbedingungen:**
1. Der USER ist eingeloggt

**Akzeptanzkriterien:**
1. Der USER findet "Account löschen" in den Einstellungen
2. Das SYSTEM zeigt einen Warnhinweis mit Konsequenzen
3. Der USER muss die Löschung explizit bestätigen (z.B. "LÖSCHEN" eintippen)
4. Das SYSTEM löscht alle personenbezogenen Daten endgültig
5. Erstellte Events werden gelöscht ODER auf "Gelöschter User" anonymisiert

**Nachbedingungen:**
1. Der USER ist ausgeloggt und kann sich nicht mehr anmelden
2. E-Mail-Adresse kann für neue Registrierung verwendet werden
3. Alle Events des Users werden **komplett gelöscht**
4. Kommentare des Users werden **anonymisiert** (Text bleibt, Author wird "Gelöschter User")

---

#### US-1.8: Daten-Export (DSGVO)
**Als** User
**möchte ich** alle meine Daten exportieren können
**damit** ich mein Recht auf Datenportabilität ausüben kann.

**Vorbedingungen:**
1. Der USER ist eingeloggt

**Akzeptanzkriterien:**
1. Der USER kann "Meine Daten exportieren" in den Einstellungen wählen
2. Das SYSTEM erstellt einen Export aller personenbezogenen Daten
3. Das SYSTEM stellt den Export als Download bereit (JSON oder ZIP)
4. Der Export enthält: Profildaten, Kreise, Events, Teilnahmen, Kommentare

**Nachbedingungen:**
1. Das SYSTEM benachrichtigt den USER wenn der Export bereit ist

---

### Non-Functional Requirements für Epic 1

| ID | Kategorie | Anforderung | Ziel | Priorität |
|----|-----------|-------------|------|-----------|
| NFR-1.1 | Security | Auth-Provider verwenden | Kein selbst-implementiertes Auth | High |
| NFR-1.2 | Security | E-Mail-Verifizierung vor Nutzung | Keine unverifizierten Accounts | High |
| NFR-1.3 | Security | Mehrere Sessions erlaubt | Keine Geräte-Beschränkung | Medium |
| NFR-1.4 | Compliance | DSGVO-konform | Löschung, Export möglich | High |
| NFR-1.5 | Usability | Social Login verfügbar | Google, Apple o.ä. | High |

---

### Constraints & Randbedingungen

1. **Auth-Provider:** Ein externer Auth-Provider (z.B. Clerk, Auth0, Supabase Auth) wird verwendet. Dieser übernimmt E-Mail-Versand, Passwort-Reset, Session-Management.
2. **Kein Admin im MVP:** Plattform-Admin-Funktionen werden später ergänzt.
3. **Eine Benutzerrolle:** Alle User haben die gleichen Berechtigungen.

---

## Epic 2: Kreise (Circles)

**Ziel:** User können Kreise erstellen, beitreten und verwalten.

### User Stories

#### US-2.1: Kreis erstellen
**Als** User
**möchte ich** einen neuen Kreis erstellen
**damit** ich Events mit einer Gruppe teilen kann.

**Vorbedingungen:**
1. Der USER ist eingeloggt
2. Der USER hat das Limit von 5 erstellten Kreisen noch nicht erreicht

**Akzeptanzkriterien:**
1. Der USER gibt einen Kreis-Namen ein (3-50 Zeichen)
2. Der USER kann optional ein Kreis-Bild hochladen
3. Der USER kann optional eine Beschreibung hinzufügen
4. Das SYSTEM prüft ob der USER das Limit von 5 erstellten Kreisen erreicht hat
5. Falls Limit erreicht: Fehlermeldung mit Hinweis auf Limit

**Nachbedingungen:**
1. Der USER ist automatisch Admin des neuen Kreises
2. Ein Einladungslink wird generiert
3. Der Kreis erscheint in "Meine Kreise"

#### US-2.2: Einladungslink generieren
**Als** Kreis-Admin
**möchte ich** Einladungslinks generieren und teilen
**damit** andere dem Kreis beitreten können.

**Vorbedingungen:**
1. Der USER ist Admin des Kreises

**Akzeptanzkriterien:**
1. Der USER kann einen Einladungslink generieren
2. Der Link kann mehrfach genutzt werden (beliebig viele Beitritte)
3. Der USER kann den aktiven Link jederzeit invalidieren
4. Der USER kann einen neuen Link generieren (ersetzt den alten)
5. Der Link kann über die Share-API geteilt werden

**Nachbedingungen:**
1. Nur der aktuell gültige Link funktioniert für Beitritte

**Out of Scope:**
1. Ablaufdatum für Links (nicht im MVP)

#### US-2.3: Kreis beitreten
**Als** User
**möchte ich** über einen Einladungslink einem Kreis beitreten
**damit** ich Events dieses Kreises sehen kann.

**Vorbedingungen:**
1. Der USER hat einen gültigen Einladungslink
2. Der USER ist eingeloggt (Link leitet zum Login falls nicht)

**Akzeptanzkriterien:**
1. Der Link öffnet die App (Deep Link) oder Webansicht
2. Der USER sieht Kreis-Name, Bild, Beschreibung und Mitgliederzahl
3. Der USER kann "Beitreten" klicken
4. Das SYSTEM prüft ob der USER bereits Mitglied ist
5. Falls bereits Mitglied: Freundliche Meldung "Du bist bereits Mitglied" und Weiterleitung zum Kreis
6. Das SYSTEM prüft ob der USER das Limit von 20 Kreisen erreicht hat
7. Falls Limit erreicht: Fehlermeldung mit Hinweis auf Limit

**Nachbedingungen:**
1. Der Kreis erscheint in "Meine Kreise"
2. Der USER sieht alle Events dieses Kreises im Feed

#### US-2.4: Kreise anzeigen
**Als** User
**möchte ich** alle meine Kreise sehen
**damit** ich einen Überblick habe.

**Akzeptanzkriterien:**
1. Liste zeigt alle Kreise mit Name und Bild
2. Sortierung nach letzter Aktivität (neuestes Event)
3. Anzeige der Mitgliederanzahl pro Kreis
4. Tap öffnet Kreis-Detail

#### US-2.5: Kreis-Mitglieder sehen
**Als** Kreis-Mitglied
**möchte ich** alle Mitglieder eines Kreises sehen
**damit** ich weiss wer dabei ist.

**Akzeptanzkriterien:**
1. Mitgliederliste zeigt Name und Profilbild
2. Admin-Rolle ist markiert
3. Tap auf Mitglied öffnet Profil-Preview

#### US-2.6: Kreis verlassen
**Als** Kreis-Mitglied
**möchte ich** einen Kreis verlassen können
**damit** ich keine Events mehr davon sehe.

**Vorbedingungen:**
1. Der USER ist Mitglied des Kreises

**Akzeptanzkriterien:**
1. Der USER findet "Kreis verlassen" in den Kreis-Einstellungen
2. Das SYSTEM zeigt einen Bestätigungsdialog mit Konsequenzen
3. Der USER kann den Vorgang abbrechen oder bestätigen
4. Der einzige Admin kann den Kreis NICHT verlassen (muss zuerst Admin-Rolle übertragen)

**Nachbedingungen:**
1. Der Kreis erscheint nicht mehr in "Meine Kreise"
2. Die Events des USERs werden aus diesem Kreis entfernt
3. Die Events bleiben beim USER und können mit anderen Kreisen geteilt werden
4. Der USER sieht keine Events mehr aus diesem Kreis im Feed

---

#### US-2.7: Kreis bearbeiten
**Als** Kreis-Admin
**möchte ich** den Kreis bearbeiten können
**damit** ich Informationen aktuell halten kann.

**Vorbedingungen:**
1. Der USER ist Admin des Kreises

**Akzeptanzkriterien:**
1. Der USER kann den Kreis-Namen ändern (3-50 Zeichen)
2. Der USER kann das Kreis-Bild ändern oder entfernen
3. Der USER kann die Beschreibung ändern oder entfernen
4. Änderungen werden sofort übernommen

**Nachbedingungen:**
1. Alle Mitglieder sehen die aktualisierten Informationen

**Out of Scope:**
1. Benachrichtigung der Mitglieder bei Änderungen

---

#### US-2.8: Mitglied entfernen
**Als** Kreis-Admin
**möchte ich** Mitglieder aus dem Kreis entfernen können
**damit** ich den Kreis verwalten kann.

**Vorbedingungen:**
1. Der USER ist Admin des Kreises
2. Das zu entfernende Mitglied ist nicht der einzige Admin

**Akzeptanzkriterien:**
1. Der USER sieht "Entfernen" Option bei jedem Mitglied (ausser bei sich selbst)
2. Das SYSTEM zeigt einen Bestätigungsdialog
3. Der USER kann den Vorgang abbrechen oder bestätigen
4. Ein Admin kann einen anderen Admin nur entfernen wenn dieser NICHT der einzige Admin ist

**Nachbedingungen:**
1. Das entfernte Mitglied sieht den Kreis nicht mehr
2. Die Events des entfernten Mitglieds werden aus dem Kreis entfernt
3. Die Events bleiben beim entfernten Mitglied (können mit anderen Kreisen geteilt werden)

**Out of Scope:**
1. Benachrichtigung des entfernten Mitglieds

---

#### US-2.9: Admin-Verwaltung
**Als** Kreis-Admin
**möchte ich** weitere Admins ernennen und meine Admin-Rolle übertragen können
**damit** der Kreis gemeinsam verwaltet werden kann.

**Vorbedingungen:**
1. Der USER ist Admin des Kreises

**Akzeptanzkriterien:**
1. Der USER kann ein Mitglied zum Admin ernennen
2. Der USER kann seine eigene Admin-Rolle an ein anderes Mitglied übertragen
3. Der USER kann einem Admin die Admin-Rolle entziehen (falls mehrere Admins existieren)
4. Das SYSTEM verhindert dass der letzte Admin seine Rolle verliert

**Nachbedingungen:**
1. Der neue Admin hat volle Verwaltungsrechte

---

#### US-2.10: Kreis löschen
**Als** Kreis-Admin
**möchte ich** einen Kreis löschen können
**damit** ich nicht mehr benötigte Kreise entfernen kann.

**Vorbedingungen:**
1. Der USER ist Admin des Kreises

**Akzeptanzkriterien:**
1. Der USER findet "Kreis löschen" in den Kreis-Einstellungen
2. Das SYSTEM zeigt einen Warnhinweis mit Konsequenzen
3. Der USER muss die Löschung explizit bestätigen (z.B. Kreis-Namen eintippen)

**Nachbedingungen:**
1. Der Kreis ist für alle Mitglieder nicht mehr sichtbar
2. Alle Mitglieder verlieren die Kreis-Mitgliedschaft
3. Events die mit diesem Kreis geteilt waren, verlieren diese Zuordnung
4. Events bleiben bei ihren Erstellern (können mit anderen Kreisen geteilt werden)
5. Events die nur mit diesem Kreis geteilt waren, sind temporär nicht sichtbar bis neu geteilt

---

#### US-2.11: Gemeinsame Kreise anzeigen
**Als** User
**möchte ich** sehen welche Kreise ich mit einem anderen User gemeinsam habe
**damit** ich den sozialen Kontext verstehe.

**Vorbedingungen:**
1. Der USER betrachtet das Profil eines anderen Users

**Akzeptanzkriterien:**
1. Das SYSTEM zeigt "Gemeinsame Kreise: Kreis A, Kreis B, ..." auf dem User-Profil
2. Nur Kreise werden angezeigt, in denen beide User Mitglied sind
3. Falls keine gemeinsamen Kreise: Keine Anzeige

---

### Non-Functional Requirements für Epic 2

| ID | Kategorie | Anforderung | Ziel | Priorität |
|----|-----------|-------------|------|-----------|
| NFR-2.1 | Limits | Max. erstellbare Kreise pro User | 5 Kreise | High |
| NFR-2.2 | Limits | Max. Kreis-Mitgliedschaften pro User | 20 Kreise | High |
| NFR-2.3 | Security | Einladungslinks nicht erratbar | UUID v4 oder ähnlich | High |
| NFR-2.4 | Usability | Kreis-Liste lädt schnell | < 1s | Medium |
| NFR-2.5 | Data Integrity | Bei Admin-Account-Löschung | Ältestes Mitglied wird Admin | High |

---

## Epic 3: Events

**Ziel:** User können Events erstellen, bearbeiten und in Kreisen teilen.

### User Stories

#### US-3.1: Event erstellen
**Als** User
**möchte ich** ein Event erstellen
**damit** ich andere zu einer Aktivität einladen kann.

**Akzeptanzkriterien:**
1. User gibt Titel ein (Pflicht, 3-100 Zeichen)
2. User gibt Datum und Uhrzeit ein (Pflicht)
3. User gibt Ort ein (Pflicht, Freitext oder Karten-Picker)
4. User kann Beschreibung hinzufügen (optional, max 2000 Zeichen)
5. User kann Bild hochladen (optional, max 10MB)
6. User wählt mindestens einen Kreis für Sichtbarkeit

#### US-3.2: Event in mehreren Kreisen teilen
**Als** Event-Ersteller
**möchte ich** ein Event in mehreren Kreisen teilen
**damit** verschiedene Freundesgruppen es sehen.

**Akzeptanzkriterien:**
1. Multi-Select für Kreise bei Event-Erstellung
2. Event erscheint in allen gewählten Kreisen
3. Kreis-Auswahl kann nachträglich erweitert werden
4. Kreise können nachträglich entfernt werden (Event verschwindet dort)

#### US-3.3: Wiederkehrendes Event erstellen
**Als** User
**möchte ich** wiederkehrende Events erstellen
**damit** ich regelmässige Treffen nicht einzeln anlegen muss.

**Akzeptanzkriterien:**
1. Wiederholungsoptionen: täglich, wöchentlich, monatlich, custom
2. End-Datum oder Anzahl Wiederholungen wählbar
3. Jede Instanz ist einzeln bearbeitbar
4. Option "alle zukünftigen bearbeiten" bei Änderungen

#### US-3.4: Event bearbeiten
**Als** Event-Ersteller
**möchte ich** mein Event bearbeiten
**damit** ich Änderungen kommunizieren kann.

**Akzeptanzkriterien:**
1. Alle Felder sind editierbar
2. Teilnehmer werden bei wesentlichen Änderungen benachrichtigt (Datum, Ort)
3. Änderungshistorie ist nicht sichtbar (Keep it simple)

#### US-3.5: Event absagen
**Als** Event-Ersteller
**möchte ich** ein Event absagen
**damit** Teilnehmer informiert werden.

**Akzeptanzkriterien:**
1. "Event absagen" Option für Ersteller
2. Event wird als "Abgesagt" markiert (nicht gelöscht)
3. Alle Teilnehmer werden per Push benachrichtigt
4. Event bleibt sichtbar mit "Abgesagt"-Banner

#### US-3.6: Event-Detail ansehen
**Als** User
**möchte ich** Event-Details sehen
**damit** ich entscheiden kann ob ich teilnehme.

**Akzeptanzkriterien:**
1. Anzeige: Titel, Bild, Datum/Zeit, Ort, Beschreibung
2. Anzeige: Ersteller mit Profilbild
3. Anzeige: Teilnehmerliste mit Status
4. Anzeige: In welchen meiner Kreise das Event geteilt ist
5. Hervorhebung: User denen ich folge (falls teilnehmend)

---

## Epic 4: Discovery & Feed

**Ziel:** User entdecken relevante Events durch einen personalisierten Feed.

### User Stories

#### US-4.1: Event-Feed anzeigen
**Als** User
**möchte ich** einen Feed mit relevanten Events sehen
**damit** ich nichts verpasse.

**Akzeptanzkriterien:**
1. Feed zeigt alle Events aus meinen Kreisen
2. Sortierung: Chronologisch nach Event-Datum (nächste zuerst)
3. Events der Vergangenheit werden ausgeblendet
4. Infinite Scroll / Pagination

#### US-4.2: Events nach Kreis filtern
**Als** User
**möchte ich** Events nach Kreis filtern
**damit** ich gezielt suchen kann.

**Akzeptanzkriterien:**
1. Filter-Dropdown mit allen meinen Kreisen
2. Multi-Select möglich
3. Filter-Status bleibt während Session erhalten

#### US-4.3: Events durchsuchen
**Als** User
**möchte ich** Events per Textsuche finden
**damit** ich gezielt nach Themen suchen kann.

**Akzeptanzkriterien:**
1. Suche durchsucht Titel und Beschreibung
2. Suche ist auf meine Kreise beschränkt
3. Ergebnisse zeigen Event-Cards

#### US-4.4: "Bekannte nehmen teil" Indikator
**Als** User
**möchte ich** sehen welche mir bekannten Personen an einem Event teilnehmen
**damit** ich ermutigt werde auch hinzugehen.

**Akzeptanzkriterien:**
1. Event-Card zeigt "X Personen denen du folgst nehmen teil"
2. Profilbilder der gefolgten Personen werden angezeigt (max 3)
3. Bei Tap auf Indikator: Liste aller mir bekannten Teilnehmer

#### US-4.5: "Aus deinen Kreisen" Indikator
**Als** User
**möchte ich** sehen aus welchen meiner Kreise Teilnehmer kommen
**damit** ich den sozialen Kontext verstehe.

**Akzeptanzkriterien:**
1. Anzeige: "Teilnehmer aus: Kreis A, Kreis B"
2. Zeigt nur Kreise in denen ich auch bin
3. Hilft bei Events die in Kreisen geteilt wurden, in denen ich nicht bin

---

## Epic 5: Following

**Ziel:** User können anderen Usern innerhalb ihrer Kreise folgen.

### User Stories

#### US-5.1: User folgen
**Als** User
**möchte ich** anderen Usern aus meinen Kreisen folgen
**damit** ich ihre Event-Teilnahmen sehe.

**Akzeptanzkriterien:**
1. "Folgen" Button auf User-Profil (nur bei gemeinsamen Kreisen)
2. Keine Bestätigung nötig (asymmetrisch)
3. Following-Liste in meinem Profil einsehbar

#### US-5.2: User entfolgen
**Als** User
**möchte ich** jemanden entfolgen können
**damit** ich meine Following-Liste pflegen kann.

**Akzeptanzkriterien:**
1. "Entfolgen" Button auf User-Profil oder in Following-Liste
2. Kein Bestätigungsdialog nötig
3. Sofortige Wirkung

#### US-5.3: Follower & Following sehen
**Als** User
**möchte ich** meine Follower und Followings sehen
**damit** ich weiss wer mir folgt.

**Akzeptanzkriterien:**
1. Tab "Follower" zeigt wer mir folgt
2. Tab "Following" zeigt wem ich folge
3. Jeweils mit Profilbild und Name

---

## Epic 6: Teilnahme & Interaktion

**Ziel:** User können an Events teilnehmen und diese kommentieren.

### User Stories

#### US-6.1: Teilnahme-Status setzen
**Als** User
**möchte ich** meinen Teilnahme-Status angeben
**damit** andere wissen ob ich komme.

**Akzeptanzkriterien:**
1. Status-Optionen: Zusage, Vielleicht, Interessiert, Absage
2. Status kann jederzeit geändert werden
3. Änderung wird im Event reflektiert

#### US-6.2: Teilnehmerliste sehen
**Als** User
**möchte ich** sehen wer an einem Event teilnimmt
**damit** ich weiss wen ich treffe.

**Akzeptanzkriterien:**
1. Gruppierung nach Status (Zusagen, Vielleicht, Interessiert)
2. Absagen werden nicht angezeigt
3. Profilbild und Name pro Teilnehmer
4. Kennzeichnung: "Folgst du" bei gefolgten Usern
5. Total-Anzahl pro Kategorie

#### US-6.3: Event kommentieren
**Als** User
**möchte ich** ein Event kommentieren
**damit** ich Fragen stellen oder mich austauschen kann.

**Akzeptanzkriterien:**
1. Kommentarfeld unter Event-Details
2. Kommentare zeigen: User, Zeit, Text
3. Chronologische Sortierung (älteste zuerst)
4. Keine Verschachtelung (flat comments)

#### US-6.4: Eigenen Kommentar löschen
**Als** User
**möchte ich** meinen eigenen Kommentar löschen können
**damit** ich Fehler korrigieren kann.

**Akzeptanzkriterien:**
1. Löschen-Option nur für eigene Kommentare
2. Bestätigungsdialog
3. Kommentar wird vollständig entfernt

#### US-6.5: Event-Kapazität und Warteliste
**Als** Event-Ersteller
**möchte ich** eine maximale Teilnehmerzahl festlegen
**damit** das Event nicht überfüllt wird.

**Akzeptanzkriterien:**
1. Optionales Feld "Max. Teilnehmer" bei Event-Erstellung
2. Bei Erreichen des Limits: "Auf Warteliste setzen" statt "Zusagen"
3. Warteliste-Position wird dem User angezeigt
4. Bei Absage eines Teilnehmers: Nächster auf Warteliste wird benachrichtigt
5. User auf Warteliste kann sich jederzeit entfernen

---

## Epic 7: Notifications

**Ziel:** User werden über relevante Aktivitäten informiert.

### User Stories

#### US-7.1: Push-Notification bei neuem Event
**Als** User
**möchte ich** benachrichtigt werden wenn ein neues Event in meinen Kreisen erstellt wird
**damit** ich nichts verpasse.

**Akzeptanzkriterien:**
1. Push bei neuem Event in abonnierten Kreisen
2. Notification zeigt: Event-Titel, Kreis, Datum
3. Tap öffnet Event-Detail

#### US-7.2: Push bei Teilnahme von gefolgten Usern
**Als** User
**möchte ich** benachrichtigt werden wenn ein User dem ich folge an einem Event teilnimmt
**damit** ich ermutigt werde auch hinzugehen.

**Akzeptanzkriterien:**
1. Push wenn gefolgter User "Zusage" oder "Vielleicht" setzt
2. Notification zeigt: "[User] nimmt an [Event] teil"
3. Tap öffnet Event-Detail

#### US-7.3: Push bei Event-Änderungen
**Als** Teilnehmer
**möchte ich** benachrichtigt werden wenn sich ein Event ändert
**damit** ich informiert bleibe.

**Akzeptanzkriterien:**
1. Push bei Änderung von Datum, Zeit oder Ort
2. Push bei Event-Absage
3. Notification zeigt Art der Änderung

#### US-7.4: Notification-Einstellungen
**Als** User
**möchte ich** meine Benachrichtigungen konfigurieren
**damit** ich nicht überflutet werde.

**Akzeptanzkriterien:**
1. Toggle pro Notification-Typ (neue Events, Following-Aktivität, Änderungen)
2. Optional: Quiet Hours (keine Push zwischen X und Y Uhr)
3. Optional: Pro-Kreis Einstellungen

---

## Epic 8: PWA & Offline

**Ziel:** Die App funktioniert als installierbare PWA mit grundlegender Offline-Fähigkeit.

### User Stories

#### US-8.1: App installieren
**Als** User
**möchte ich** die App auf meinem Homescreen installieren
**damit** ich schnell darauf zugreifen kann.

**Akzeptanzkriterien:**
1. PWA-Manifest ist korrekt konfiguriert
2. "Zum Homescreen hinzufügen" Prompt erscheint
3. App startet im Standalone-Modus

#### US-8.2: Offline-Zugriff auf Events
**Als** User
**möchte ich** meine Events auch offline sehen können
**damit** ich unterwegs nicht ohne Info bin.

**Akzeptanzkriterien:**
1. Zuletzt geladene Events werden gecacht
2. Offline-Indikator zeigt fehlende Verbindung
3. Aktionen werden gequeued und bei Verbindung synchronisiert

#### US-8.3: Push-Notifications empfangen
**Als** User
**möchte ich** Push-Notifications empfangen auch wenn die App geschlossen ist
**damit** ich nichts verpasse.

**Akzeptanzkriterien:**
1. Service Worker registriert für Push
2. Notification-Permission wird angefragt
3. Push funktioniert im Hintergrund

---

## Non-Functional Requirements

| ID | Kategorie | Anforderung | Ziel | Priorität |
|----|-----------|-------------|------|-----------|
| NFR-1 | Performance | Feed lädt schnell | < 2s initial load | High |
| NFR-2 | Performance | Event-Liste scrollt flüssig | 60fps, kein Jank | High |
| NFR-3 | Usability | Mobile-first Design | Touch-optimiert | High |
| NFR-4 | Usability | Intuitive Navigation | Max 3 Taps zu jeder Funktion | High |
| NFR-5 | Security | Daten nur für Kreis-Mitglieder | Kein Zugriff ohne Mitgliedschaft | High |
| NFR-6 | Security | Einladungslinks nicht erratbar | UUID v4 oder ähnlich | High |
| NFR-7 | Availability | App verfügbar | 99.5% uptime | Medium |
| NFR-8 | Scalability | Wachstum unterstützen | 10k User, 1k Events/Monat initial | Medium |
| NFR-9 | Localization | Mehrsprachigkeit | DE/EN initial | Medium |

---

## Weitere geklärte Entscheidungen

| Thema | Entscheidung |
|-------|-------------|
| Moderation | Nicht im MVP – Vertrauen in Community |
| Max Teilnehmer | Optional mit Warteliste-Funktion |
| User-Löschung | Events löschen, Kommentare anonymisieren |

## Offene Fragen (noch zu klären)

1. ~~**Kreis-Löschung:** Was passiert mit Events wenn ein Kreis gelöscht wird?~~ → Geklärt: Events bleiben beim User, verlieren nur Kreis-Zuordnung (siehe US-2.10)
2. ~~**Max Kreise:** Gibt es ein Limit wie vielen Kreisen ein User beitreten kann?~~ → Geklärt: Max 5 erstellen, max 20 beitreten (siehe NFR-2.1, NFR-2.2)
3. ~~**Private Events:** Soll es Events geben die nur für bestimmte Mitglieder eines Kreises sichtbar sind?~~ → Geklärt: Nein. "Open Events" = alle Events sind für alle Mitglieder der geteilten Kreise sichtbar

---

## Empfohlene MVP-Priorisierung

### MVP (v1.0)
- Epic 1: User Management (vollständig)
- Epic 2: Kreise (vollständig)
- Epic 3: Events (US-3.1, US-3.2, US-3.4, US-3.5, US-3.6 – ohne wiederkehrende Events)
- Epic 4: Discovery (US-4.1, US-4.2, US-4.4)
- Epic 6: Teilnahme (US-6.1, US-6.2 – ohne Kommentare)
- Epic 8: PWA (US-8.1)

### v1.1
- Epic 3: Wiederkehrende Events (US-3.3)
- Epic 5: Following (vollständig)
- Epic 6: Kommentare (US-6.3, US-6.4)
- Epic 7: Notifications (vollständig)

### v1.2
- Epic 4: Suche (US-4.3), Kreis-Indikator (US-4.5)
- Epic 8: Offline & Push (US-8.2, US-8.3)
- Karten-Integration (optional)

---

## Nächste Schritte

1. **Offene Fragen klären** (siehe oben)
2. **Wireframes/Mockups** erstellen für Hauptflows
3. **Tech Stack** entscheiden (Frontend, Backend, DB)
4. **MVP Stories** detaillieren mit Akzeptanzkriterien
5. **Architektur-Design** erstellen
