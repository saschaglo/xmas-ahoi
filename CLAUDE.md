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
- Domain terms live in the German glossary `CONTEXT.md` (for example "Crew", "Rätsel", "Punktespiel"). Use them exactly; avoid the terms it lists under _Avoid_.
- Points that affect several games belong in `concepts/spiele/uebergreifend.md`, not in each game page.

## Open concept work

Open decisions are tracked as a wayfinder map in `.scratch/gesamtkonzept/map.md`, with one ticket per decision under `issues/`. Resolving a ticket means writing the decision into the affected `concepts/` page, removing the open point there, and appending a line to the map's "Decisions so far".

## Core model (spans several concept files)

```
Bordkarte (QR + 4-char code) -> Check-in -> Schatzkarte with 6 stations
  -> per station: solo or Crew formed on the spot (max 3, dissolves after)
  -> finished station = 1 key + 0..100 points; Abbruch = no key, replay
  -> 6 keys -> "Schatz bergen" -> Lostopf -> ranking by points at the award ceremony
```

- Game state belongs to the player and lives on the server. Crews exist for one station only, and there are no crew accounts.
- Hints, exits, and aborts are logged, idempotent, and visible to the admin in the ranking.
- Rätsel have no time limit (except Morsezeichen: 5 min, then an automatic exit) but 2 hints per Crew and an exit. The first hint unlocks 0:30 after the run starts, each hint locks the next for 60 s, and the exit opens 60 s after hint 2 (earliest 2:30). A hint's text shows only to the player who took it (Morsezeichen: to the whole Crew). Punktespiele have a time limit and neither hints nor exit. See `concepts/framework/hinweise-abbruch-exit.md`.

## Commands

There is no build yet. Check the relative Markdown links:

```sh
for f in $(find . -name '*.md' -not -path './.claude/*'); do d=$(dirname "$f"); grep -o '](\([^)#]*\.md\)' "$f" | sed 's/](//' | while read -r l; do [ -f "$d/$l" ] || echo "BROKEN $f -> $l"; done; done
```

## Git

Commits are GPG-signed. On the maintainer's machine, the local and uncommitted `.claude/settings.local.json` excludes `git *` from the Bash sandbox so git can reach `~/.gnupg`. The exclusion applies only when every segment of the call is a git command and nothing in it uses `$(…)`. A pipe into another command, a leading non-git command, or any `$(…)` runs the call sandboxed, and signing fails with `No secret key`. That includes the `git commit -m "$(cat <<'EOF' … EOF)"` heredoc form, so pass multi-line messages as repeated `-m` arguments. If signing still fails, stage the files and give the user the commit command to run with the `!` prefix. Never bypass signing with `--no-gpg-sign`.
