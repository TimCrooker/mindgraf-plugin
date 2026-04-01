---
name: research
description: >
  Web research that feeds the MindGraf knowledge graph. Use when the user asks to "research X",
  "look up Y", "summarize this URL", "deep dive on Z", "what do we know about", "enrich this
  entity", or wants to find and capture external information.
---

# Research Skill

Research $ARGUMENTS and convert findings into linked, source-attributed graph updates.

## Invocation
```
do shell script "export MINDGRAF_PATH=${user_config.mindgraf_path} && ${user_config.mindgraf_path}/apps/cli/bin/brain <command> 2>&1"
```

## Non-Negotiable Rules
1. Graph first: always `brain search` before creating.
2. Synthesis only: never copy source text verbatim.
3. Source attribution: every claim maps to sources.
4. Write gate: present planned mutations, get approval before writing.
5. Use `--json` when parsing output.

## Entity Mapping
- Concepts/theories -> `note` (tag `concept`)
- People -> `person`
- Companies -> `organization`
- Products/platforms -> `tool`
- Articles/papers/videos -> `resource`
- Tradeoff choices -> `decision`

## Workflows

### Quick Research
Trigger: "research X", "look up Y"
1. `brain search "<topic>" --json` — check graph
2. Web search 2-4 angles, fetch top sources
3. Present planned creates/updates/links -> get approval
4. Execute approved writes
5. Report entity refs + source summary

### Deep Research
Trigger: "deep dive", "investigate"
1. Plan scope, subtopics, source strategy -> get approval
2. Per subtopic: fetch, extract, map to entities
3. Present mutations -> get approval
4. Execute, create synthesis note if needed
5. Final report

### Summarize URL
Trigger: "summarize URL", "capture URL", "save URL"
1. Fetch URL, extract metadata
2. `brain search "<url>" --type resource --json` for dedup
3. Plan -> approve -> create/update resource with synthesis

### Enrich Existing Entity
Trigger: "research more about [[X]]"
1. `brain read "<ref>" --json` — identify gaps
2. Gather targeted sources
3. `brain edit "<ref>" --append "<block>"` after approval

## Output Contract
1. Summary — 3-6 bullet synthesis
2. Key Facts — with source markers [S1]
3. Open Questions
4. Sources — numbered with URL and date

## Definition of Done
- Dedupe performed, source attribution present, writes approved, links/tags applied
