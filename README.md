# Kupido

Kupido ist ein Online-Kleinanzeigen-Marktplatz nach dem Vorbild von willhaben.
Registrierte Benutzer stellen gebrauchte Artikel als Anzeigen ein. Andere Benutzer suchen und filtern Anzeigen und kontaktieren den Verkäufer direkt.

Zahlung, Versand und Käuferschutz gehören nicht zum Umfang. Käufer und Verkäufer regeln das selbst.

Schulprojekt im Fach SYP (4AHINF), umgesetzt von einem 5er-Team mit einem agentischen, spec-getriebenen Workflow (OpenSpec + Claude Code + Codex + Freebuff).

## Tech-Stack

| Bereich     | Technologie                                   |
|-------------|-----------------------------------------------|
| Frontend    | Angular (TypeScript)                          |
| Backend     | ASP.NET Core Web API (C#), REST/JSON          |
| Datenbank   | PostgreSQL mit Entity Framework Core (Code-First, Migrations) |
| Tests       | xUnit (Backend), Angular-Tests (Frontend)     |
| Betrieb     | docker-compose                                |

## Starten

Voraussetzung: Docker mit Compose-Plugin.

```bash
docker compose up
```

Damit starten Frontend, Backend und PostgreSQL gemeinsam. Jede Änderung muss so lauffähig bleiben.

> Die Anwendung ist noch im Aufbau. Ports und Umgebungsvariablen werden hier ergänzt, sobald `docker-compose.yml` existiert.

## Module

| Kürzel    | Inhalt                                                   |
|-----------|----------------------------------------------------------|
| `AUTH`    | Registrierung, Login, Session                            |
| `USER`    | Profil, Konto                                            |
| `LISTING` | Anzeigen erstellen, bearbeiten, löschen; Bilder, Kategorien |
| `SEARCH`  | Suche, Filter, Sortierung                                |
| `CONTACT` | Kontakt und Nachrichten an den Verkäufer                 |
| `ADMIN`   | Moderation                                               |

## Spec-getriebener Workflow

Die Specs unter `openspec/specs/` sind die Quelle der Wahrheit. Code folgt den Specs, nicht umgekehrt.
Projektkontext und Regeln für die Agenten stehen in [`openspec/config.yaml`](openspec/config.yaml).

Ablauf pro Feature:

1. **Issue anlegen** auf GitHub.
2. **Branch erstellen** für genau dieses Issue.
3. **Change vorschlagen:** `/openspec-propose`. Es entstehen proposal, design, specs und tasks unter `openspec/changes/<name>/`. Der Change-Name entspricht dem Branch-Namen ohne Präfix.
4. **Umsetzen:** `/openspec-apply-change` arbeitet die Tasks ab.
5. **PR gegen `main`** mit `Closes #<Issue>` und den betroffenen REQ-IDs.
6. **Archivieren** nach dem Merge: `/openspec-archive-change`. Die Delta-Specs werden in `openspec/specs/` übernommen.

Grundregel: **1 Issue = 1 Branch = 1 PR = 1 OpenSpec-Change.**

### Anforderungs-IDs

Jede Anforderung hat eine stabile ID im Format `REQ-<MODUL>-<Nr>`, z. B. `REQ-AUTH-001`.
IDs werden nie umnummeriert oder wiederverwendet. Neue Anforderungen bekommen die nächste freie Nummer im Modul.

In den Specs:

```markdown
### Requirement: REQ-LISTING-003 Anzeige bearbeiten
Der Verkäufer SHALL eigene Anzeigen bearbeiten können.

#### Scenario: Preis ändern
- **WHEN** der Verkäufer den Preis seiner Anzeige ändert
- **THEN** zeigt die Anzeige den neuen Preis
```

### Tests und Rückverfolgbarkeit

Jede Anforderung ist durch mindestens einen automatisierten Test abgedeckt.

Backend (xUnit):

```csharp
[Fact]
[Trait("Req", "REQ-AUTH-001")]
public async Task Register_WithValidData_CreatesUser() { ... }
```

Tests für eine Anforderung gezielt ausführen:

```bash
dotnet test --filter "Req=REQ-AUTH-001"
```

Frontend (Angular): die ID steht im Testtitel, z. B. `it('REQ-SEARCH-002: filtert nach Kategorie', ...)`.

## Konventionen

- OpenSpec-Artefakte (proposal, design, specs, tasks) auf **Deutsch**.
- Code, Bezeichner, API-Routen, Commit-Messages und Branch-Namen auf **Englisch**.
- Schemaänderungen nur über EF-Core-Migrations.

## Projektstruktur

```
openspec/
  config.yaml      Projektkontext und Regeln für die Agenten
  specs/           aktuelle Specs (Quelle der Wahrheit)
  changes/         offene Changes; archive/ für abgeschlossene
.claude/skills/    OpenSpec-Skills für Claude Code
.agents/skills/    OpenSpec-Skills für andere Agenten
```
