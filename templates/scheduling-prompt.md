Run the `news-digest` skill for slot=`{{slot}}`.

Workspace: `{{workspace_path}}`

Behavior:

- If `news-digest-data/settings.yaml` is missing or has `init.complete: false`, do not run. Reply with a single line: "news-digest setup incomplete; manual run required." and stop.
- Otherwise, execute the full collection workflow:
  - load `settings.yaml`, `seen.jsonl`, `feedback.jsonl`, recent digests, and `templates/`
  - collect candidates per topic (primary sources, community, media, social, web search)
  - apply feedback and source policy to ranking
  - deduplicate semantically against `seen.jsonl` and recent digests
  - render the digest using `templates/`
  - write `news-digest-data/{{date}}/{{slot}}.md`
  - append accepted items to `seen.jsonl`

Output contract:

- Reply with the exact contents of the written markdown file. No preface. No summary. No closing remarks. No tool-use commentary.
- If nothing qualifies, reply with the rendered `empty.md` for that slot.
- Do not propose source/feedback changes in this reply — defer those to the next interactive run.
