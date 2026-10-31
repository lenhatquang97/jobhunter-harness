# Accepted decisions

Recorded for this harness on **2026-10-30**. These decisions describe scope and direction; they do not prove implementation or grant authority beyond the current assignment.

| Decision | Consequence |
| --- | --- |
| Keep a separate `jobhunter-harness` repository with no application codebase | Store Codex instructions, context, skills, native subagent definitions, progress, hooks and small helper scripts here; implement application behavior in its owning repository |
| Extend the existing Odoo internal app through a new addon | Reuse Odoo data, identity and access controls; integrate with the Python backend instead of creating a standalone replacement app |
| Use two interview modes | Quick AI screening conducts a bounded first-round conversation; the human-to-human assistant transcribes/translates an interview and suggests follow-ups and feedback |
| Ground quick screening in Company context, JD and CV | Ask basic role, experience, motivation, salary, availability and work-arrangement questions; check candidate self-reports against the supplied CV/JD and preserve aligned, conflicting or unverified findings with answer references |
| Keep the current speech-provider split | Quick AI screening currently uses OpenAI Realtime for English voice; the human-to-human assistant uses AssemblyAI realtime transcription/translation. Verify actual API/account capabilities and report failures rather than silently substituting providers |
| English voice; English and Vietnamese typed participation | Require audio consent. Auto-submit a final candidate voice segment after 3.5 seconds of silence, reset the timer when speech resumes, and keep manual submission as backup; typed answers remain reviewable and explicitly submitted |
| Keep interview evidence human-auditable | Exclude agent playback/echo from candidate evidence; ground follow-ups and feedback in the final transcript, rubric and referenced answers; do not score, rank or make hiring decisions |
| Future AWS pipeline targets backend on ECS Fargate | Put pipeline/infrastructure implementation in the backend application repository; Odoo deployment and live AWS changes are outside harness setup |
| Use native Codex development subagents and skills | Keep `explorer`, `implementer` and `reviewer` distinct from runtime recruiting roles; inherit the parent model and actual permissions |
| Use revision-checked `progress.json` | Track engineering tasks and acceptance evidence; application run/candidate state belongs in the application |
| Hook activation requires host review/trust | Ship reviewable definitions; activate through the normal project trust flow and `/hooks`, without bypassing trust |

The Notion shared/role snapshots define the recruiting constraints summarized in `product-contract.md`. The current interview implementation profile above changes only the provider split and the explicit-submit-only rule for final **voice screening** segments. Typed interview answers still require explicit submission. All shared authority rules remain unchanged: specialists do not rank or decide candidates, change stages, send outreach or book meetings. Older repository plans still inform development conventions and history; their automatic decisions, calendar/email execution or standalone speech design are not accepted expansions of the reviewed product scope.

When a later user decision changes this direction, record the concrete change and affected task/source references. Do not rewrite observed historical test results or quietly turn an assumption into a decision.
