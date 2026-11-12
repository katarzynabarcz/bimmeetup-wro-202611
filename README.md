---
title: "README"
type: "readme"
audience: "human-only"
created: "2026-11-12"
description: "Human guide to the workshop starter: what it contains and the order of steps from setup to a working agent project."
tags:
  - readme
  - starter-template
---

# Starter środowiska agentowego

Pusty starter, z którego z pomocą AI budujesz własny system agentowy pod swoje zadanie. Działa w Antigravity, Claude Code, Codex i innych narzędziach, które czytają `AGENTS.md`. Nie ma tu konfiguracji pod konkretny system ani narzędzie: agent ustawia ją sam, na Twoim komputerze, według oficjalnej dokumentacji swojego narzędzia.

To jedyny README w starterze. Agent go nie czyta, jest dla Ciebie.

## Co jest w środku

| Ścieżka | Co to jest |
|---|---|
| `AGENTS.md` | Główna instrukcja dla agenta. Wspólna dla wszystkich narzędzi. |
| `START-PROMPT.md` | Jedno zdanie do wklejenia w pierwszej rozmowie. |
| `setup/SETUP-GOALS.md` | Jak ma wyglądać środowisko po ustawieniu. Cel, nie instrukcja krok po kroku. |
| `setup/GOOD-PRACTICES.md` | Dobre praktyki dla AGENTS.md, reguł, skilli, subagentów i MCP w każdym narzędziu. Agent czyta je obok oficjalnej dokumentacji. |
| `setup/patterns/` | Wzory reguły, skilla i subagenta oraz dwie przykładowe reguły. Nieaktywne, służą do kopiowania. |
| `.agents/rules/` | Twoje reguły: standardy i ograniczenia. Na starcie pusty. |
| `.agents/skills/` | Twoje skille: procedury krok po kroku, opcjonalnie ze skryptami. Na start `project-setup`. |
| `.agents/agents/` | Twoi subagenci. Na start `environment-builder`, architekt projektu. |
| `knowledge/Project-Intake.md` | Ankieta Twojego projektu. Wypełniasz ją sam. |
| `knowledge/` | Fakty o projekcie, słownik, zapis środowiska (`Environment.md`). |
| `knowledge/changelog/` | Historia zmian po każdej sesji. Dzięki niej nowa rozmowa albo inne narzędzie wie, na czym skończyłeś. |
| `templates/` | Wzory dokumentów wynikowych. |
| `project/` | Pliki Twojego projektu: dane wejściowe, wersje robocze, wyniki. Podfoldery zaprojektuje `environment-builder`. |

## Jak zacząć

### Krok 0–1. Ustawienie środowiska

1. Rozpakuj starter i otwórz folder w swoim narzędziu (Antigravity, Claude Code, Codex…).
2. Wklej zdanie z `START-PROMPT.md`.
3. Agent sam sprawdzi system i narzędzie, a Ty tylko potwierdzisz.
4. Agent powie prostymi słowami, co chce utworzyć lub podłączyć. Zatwierdź albo popraw.
5. Agent zapisze wynik w `knowledge/Environment.md`, zrobi pierwszy wpis w ChangeLogu i poprosi o nową rozmowę. Poda zdanie, od którego ją zaczniesz:

```text
Skończyłem ustawianie środowiska. Jaki jest mój następny krok?
```

### Krok 2. Ankieta projektu

1. W nowej rozmowie wklej to zdanie. Agent zaprosi Cię do wypełnienia `knowledge/Project-Intake.md`.
2. Wypełnij wszystkie 10 pól, konkretnie: nazwy plików, liczby, przykłady. Nagłówkiem i statusem się nie przejmuj, obsługuje je agent.
3. Gdy skończysz, napisz agentowi: „Skończyłem”.

Agent nie zadaje tych pytań w czacie. Jeśli zostało puste pole, powie które.

### Krok 3. Projekt i budowa

1. Po „Skończyłem” agent oznaczy ankietę jako gotową i przekaże pracę do `environment-builder`.
2. Ten przeczyta ankietę i dopyta, jedno pytanie naraz, tam gdzie napisałeś zbyt ogólnie.
3. Opowie, jak będzie wyglądał Twój projekt i co po drodze zatwierdzisz. Zatwierdź albo popraw.
4. Buduje po kawałku, zaczynając od minimum z pytania 10. Po każdym kroku czeka na Twoją akceptację.

### Dalsza praca

- Agent sam prowadzi ChangeLog: jeden krótki plik na dzień, w punktach (zrobione, dalej, decyzje, blokery). Możesz też poprosić: „zapisz changelog”.
- Nowa reguła, skill albo subagent: poproś `environment-builder`. Weźmie wzór z `setup/patterns/`.
- Skrypty Pythona działają w środowisku wirtualnym `.venv/`. Agent założy je przy pierwszym skrypcie, po Twojej zgodzie.

## Jakie modele wybrać na pierwsze kroki

Stan z 12.11.2026, według oficjalnych list modeli. Przed warsztatem sprawdź, co faktycznie widać w selektorze: listy zmieniają się co kilka tygodni i zależą od planu.

Zasada: **myślenie i projektowanie** (setup, ankieta, dopytywanie, plan projektu) na mocnym modelu i wyższym wysiłku. **Wykonywanie zatwierdzonego planu** (zakładanie plików, ChangeLog) na tańszym modelu i średnim wysiłku.

### Claude Code

| Etap | Model | Effort |
|---|---|---|
| Setup, ankieta, grill-me, plan projektu | Opus 5.5 (`opus`) | `high` |
| Budowa krok po kroku | Sonnet 5.5 (`sonnet`) | `medium` |
| Proste, powtarzalne zadania (konwersje, porządki) | Haiku 5.5 (`haiku`) | `low` |

Dostępne poziomy: `low`, `medium` (domyślny), `high`, `xhigh`, `max`. `xhigh` i `max` zostaw na trudne problemy, bo szybko zużywają limit.

### Antigravity

| Etap | Model | Thinking |
|---|---|---|
| Setup, ankieta, grill-me, plan projektu | Gemini 4 Hydrogen (wszystkie plany) | High |
| Alternatywa w planie Pro/Ultra | Gemini 4 Argon albo Claude Opus 5.5 (Thinking) | High |
| Budowa krok po kroku | Gemini 3.8 Flash (wszystkie plany) | Medium |

Hydrogen ma też poziom Extra, ale do pierwszych kroków nie jest potrzebny. Claude Sonnet 4.6 i Opus 4.6 znikają z Antigravity 18.11.2026, więc ich nie polecaj.

### Codex

| Etap | Model | Reasoning effort |
|---|---|---|
| Setup, ankieta, grill-me, plan projektu | GPT-6.1 Sol (`gpt-6.1-sol`) | Medium |
| Bardzo trudny projekt albo plan nie wychodzi | Astra (`gpt-6-astra`) | Medium |
| Budowa krok po kroku | GPT-6.1 Sol | Light (`low`) |
| Masowe, powtarzalne zadania | GPT-6 Luna (`gpt-6-luna`) | High |

Uwaga: subagent w Codexie bez pola `model` dziedziczy model z głównej rozmowy. Wybór modelu w czacie dotyczy więc też `environment-builder`.

Źródła: [Claude Code – model config](https://code.claude.com/docs/en/model-config) · [Antigravity – models](https://antigravity.google/docs/models) · [Codex – models](https://learn.chatgpt.com/docs/models)

## Zasady, których pilnuje agent

- Każda treść jest w jednym miejscu, w `.agents/`. Pliki konkretnego narzędzia tylko do niej odsyłają.
- Konfigurację narzędzia opiera na jego oficjalnej dokumentacji, nie na domysłach.
- Nie zmienia nic poza folderem projektu bez Twojej zgody.
- Każdy wynik jest wersją roboczą (DRAFT), dopóki go nie zatwierdzisz.
