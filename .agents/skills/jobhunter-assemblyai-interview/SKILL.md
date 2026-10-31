---
name: jobhunter-assemblyai-interview
description: Build or review Jobhunter quick AI screening and human interview assistance, including OpenAI Realtime screening voice, AssemblyAI realtime transcription/translation, evidence-grounded follow-ups and mode-specific answer submission.
---

# Jobhunter interview modes

Implement in the owning application repositories identified by [project context](../../../context/project.md). Read the [product contract](../../../context/product-contract.md), relevant [decisions](../../../context/decisions.md), and [capabilities](../../../context/capabilities.json). This skill does not create an application in the development-harness repository.

## Workflow

1. Inspect the actual interview UI, backend, consent model, provider adapters, evidence persistence and tests. Identify whether the bounded Orchestrator assignment targets quick AI screening or the human-to-human interview assistant; do not mix their provider or submission rules. Interview does not dispatch other specialists or own overall request status.
2. For **quick AI screening**, retrieve from Company context, JD and CV and conduct a bounded first-round interview covering relevant basics such as current role, experience, motivation, salary expectation, availability and work arrangement. Use the current OpenAI Realtime integration for English voice and support English/Vietnamese typed answers.
3. For the **human-to-human interview assistant**, use AssemblyAI realtime transcription/translation for the interview audio and generate follow-up suggestions and final feedback from the attributed final transcript and rubric. Confirm current provider API/SDK and account capabilities from official documentation and observed results; a documentation link is not proof of account access.
4. Keep provider API keys server-side. Mint short-lived, authorized browser session credentials where supported. Bind speech sessions to the actual interview/session context and expose capability failures as actionable errors.
5. Obtain explicit consent before collecting or processing candidate audio. When consent is absent or withdrawn, stop further audio collection/processing and keep typed participation and question preparation available. Do not infer consent from microphone permission or from a transcript being present.
6. In quick screening, keep partial STT text as a draft. When a final candidate segment arrives, start a 3.5-second silence countdown; reset it if new candidate speech arrives, then submit once after uninterrupted silence. Manual submission remains a backup, and typed answers require explicit submission. A disconnect or provider end-of-turn event by itself must not fabricate or duplicate an answer.
7. Use verified speaker attribution. Do not assume the first diarization label identifies the interviewer. Keep agent speech/playback/echo separate from candidate evidence.
8. Implement microphone capture, streamed playback, interruption/cancellation and connection cleanup using supported browser APIs. Preserve submitted answers across reconnects; never label an unanswered or interrupted interview conducted or complete.
9. Generate quick-screening follow-ups and findings from approved criteria, supplied Company/JD/CV context and submitted answers. Treat CV comparison as consistency checking only: preserve aligned, conflicting and unverified findings with question/answer/source references. Generate human-assistant follow-ups and feedback from the final attributed transcript and rubric. Missing coverage is unknown. Do not assign candidate or criterion scores, rank candidates, make hiring decisions, or derive emotion, personality or suitability from voice.
10. Return the shared typed handoff with assignment/context, result (`Complete`, `Needs input` or `Failed`), output references or a clearly labeled unsaved draft, open items and next action/owner. Verify consent, typed-only participation, English screening voice, silence auto-submit timing/reset/deduplication, manual backup, AssemblyAI transcription/translation, speaker separation, interruption, provider rejection/disconnect, resume and source freshness. Keep fixtures explicitly labeled and distinguish offline tests from observed provider behavior.

Use actual available tools; the capability registry does not declare callable tool names. Return results and blockers to the coordinator, which alone updates progress with `.venv/bin/python scripts/harness.py progress update`.

## Sources

Original local workflow; no third-party skill package is vendored. Source links checked on **2026-10-30**. AssemblyAI references apply to the human interview assistant; verify current provider contracts during implementation. The quick-screening provider split and silence auto-submit behavior reflect the current user decision recorded in `context/decisions.md`.

- [AssemblyAI browser STT/TTS integration](https://www.assemblyai.com/blog/build-a-voice-assistant-app-with-assemblyai)
- [AssemblyAI streaming TTS in Python](https://www.assemblyai.com/blog/text-to-speech-python)
- [Project source registry](../../../context/sources.json)
