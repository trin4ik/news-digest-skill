# news-digest skill

## Layout

```
news-digest/
  SKILL.md                          # main skill file
  templates/
    item.md                         # single news item template
    digest.md                       # digest wrapper template
    empty.md                        # used when nothing qualifies
    scheduling-prompt.md            # prompt template for external schedulers
  defaults/
    settings.yaml                   # default settings for fresh install
  README.md                         # this file
```

## Install

1. Drop the `news-digest/` folder (the entire skill, **excluding** `README.md` if you want a clean install) into your skills directory.
2. On first invocation, the skill creates `<workspace>/news-digest-data/` and runs the setup wizard. The default `settings.yaml` and templates are copied from the skill folder.

Minimum files for a working skill install:

```
news-digest/
  SKILL.md
  templates/{item,digest,empty,scheduling-prompt}.md
  defaults/settings.yaml
```

## Migrating from v1 (Trin)

If you already have a `news-digest-data/settings.yaml` on schema version 1:

1. Back up the existing `settings.yaml`.
2. The migrated config has `init.complete: true`, so the skill skips the wizard and goes straight to collection.
3. Existing `seen.jsonl` and `source-notes.md` are kept as-is.
4. `feedback.jsonl` will be created on first feedback event — no action needed.
5. Templates: copy `templates/*.md` from the skill folder into `news-digest-data/templates/` once. Edit them to taste.

Changes from v1 to v2:

- Removed top-level `schedule:` (skill no longer manages cron).
- Removed `output.item_format` (rendering driven by templates).
- Removed `source_policy.use_configured_sources_as_hints_not_limits` (always true).
- Added `init` (onboarding state).
- Added `retention.feedback_compaction_months`.
- Added per-topic `blocked_sources`, `boost_keywords`, `penalty_keywords`.
- Added `feedback.jsonl` (append-only feedback log).
- Added `templates/` (digest rendering).

## Scheduling

The skill never registers cron jobs itself. To get scheduled digests, ask the skill "помоги настроить расписание" / "help me set up scheduling" — it renders `templates/scheduling-prompt.md` with your slot/time choices and gives you a prompt to install in your own scheduler (OpenClaw cron, system cron + a CLI agent, etc.).
# news-digest-skill
