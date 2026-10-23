# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

"Xmas Ahoi!" is an unsupervised treasure hunt for a company Christmas party on 20 November 2026. Players use a mobile website (not an app) built with Nuxt and hosted on the internet. There is no code yet. The concept is being finalized first, and implementation starts after that.

| Folder | Content | Language |
|--------|---------|----------|
| `concepts/` | Business concept: rules, flows, admin, minigames | German |
| `project/` | Technical implementation (Nuxt) | English is fine |

`concepts/README.md` is the entry page. Every concept file must be reachable from it through relative links.

## Source of truth

- `concepts/` is authoritative. It was transferred from the Trilium note "Projekt Xmas Ahoi – Schatzsuche Weihnachtsfeier" (note ID `pIpqJQVMLIWQ`, reachable via the `trilium` MCP server).
- Trilium still has outdated values that were corrected here: Laravel became **Nuxt**, and solved stations show a **key** (Schlüssel), not a check mark.
- Trilium's "Archiv" subtree is an old state. It is reference material only, and its content must never flow into the concept. `concepts/archiv/archiv.md` only lists those pages by note ID.

## Concept conventions

- Every concept file starts with `Quelle: <Trilium-Seite> · Stand <Datum> · [Übersicht](…)` below the title.
- Open points go in a section named `Offene Punkte` or `Offene Fragen und Widersprüche`. Do not delete an open point without the user's decision; when a decision is made, write it into the body and remove the point.
- Game pages separate the organizer's input from suggestions. `Beschreibung` and `Variationen` hold the organizer's input. `Aufbau (Empfehlung)` and `Offene Fragen` hold review suggestions that are not decided yet.
- Domain terms live in the German glossary `CONTEXT.md` (for example "Crew" vs. "Besetzung"). Use them exactly; avoid the terms it lists under _Avoid_.
- Points that affect several games belong in `concepts/spiele/uebergreifend.md`, not in each game page.

## Open concept work

Open decisions are tracked as a wayfinder map in `.scratch/gesamtkonzept/map.md`, with one ticket per decision under `issues/`. Resolving a ticket means writing the decision into the affected `concepts/` page, removing the open point there, and appending a line to the map's "Decisions so far".

## Core model (spans several concept files)

```
Bordkarte (QR + 4-char code) -> Check-in -> solo or Crew (max 3, lobby)
  -> Schatzkarte with 6 stations -> each solved station = 1 key
  -> 6 keys -> "Schatz bergen" -> Lostopf -> 3 random draws via admin panel
```

- Game state belongs to the player and lives on the server. Crews only synchronize play together, and there are no crew accounts.
- Hints, exits, and skipped keys are logged, idempotent, and visible to the admin during the draw.
- Unresolved: the global minimum times for hints and exit (1:30 / 3:00 / 4:00) do not fit timed games such as the quiz. See `concepts/spiele/uebergreifend.md`.

## Commands

There is no build yet. Check the relative Markdown links:

```sh
for f in $(find . -name '*.md' -not -path './.claude/*'); do d=$(dirname "$f"); grep -o '](\([^)#]*\.md\)' "$f" | sed 's/](//' | while read -r l; do [ -f "$d/$l" ] || echo "BROKEN $f -> $l"; done; done
```

## Git

Commits are GPG-signed. The Bash sandbox cannot read `~/.gnupg`, so `git commit` fails inside it. Stage the files, then give the user the exact commit command to run with the `!` prefix. Never bypass signing with `--no-gpg-sign`.
