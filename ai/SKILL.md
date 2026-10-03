---
name: holodori-web-assistant
description: Use an open HolodoriCalc browser tab to calculate team scores, interpret results, and compare growth scenarios through browser controls or available site tools.
---

# HolodoriCalc web assistant

Let the website perform all calculations. This skill supplies instructions only: no engine, card/chart database, executable scripts, or calculation server. Answer in Korean unless the user requests another language.

## Connect

Use the user's existing calculator tab in the browser profile where they normally use the calculator, including Edge with the official ChatGPT browser extension. The extension connects to the desktop app and can open side chat from its toolbar icon. Data saved in that browser profile is used directly; do not ask the user to move it to the built-in browser. Respect a supplied localhost or other calculator URL. If no tab is available, open https://doricalc-holo.live/ in that connected browser. If browser control itself is unavailable, explain the required desktop-app/browser connection; reading this guide does not provide that capability. Do not promise ordinary ChatGPT web/mobile support.

For Windows setup guidance, point to the [official ChatGPT Windows installer](https://get.microsoft.com/installer/download/9PLM9XGG6VKS?cid=website_cta_psi), then have the user open the app and sign in. The [official browser extension](https://chromewebstore.google.com/detail/chatgpt/hehggadaopoacecdllhhajmbjkdcmajg?hl=ko) is installed in their usual calculator browser profile. The desktop app's **Settings → Computer Use** shows the browser connection; see the [official connection instructions](https://learn.chatgpt.com/docs/chrome-extension) if setup is incomplete. Do not install software merely to answer a calculator analysis request.

Choose the available route:

- **Browser controls:** Read [Browser calculation workflow](#browser-calculation-workflow) before operating the calculator. This route uses the existing calculation screens and needs no AI connection switch. An unavailable WebMCP capability or disabled **이 탭에서 AI 연결 켜기** switch does not prevent screen interaction. Do not repeatedly attempt an unsupported connection.
- **Site tools:** Prefer them only when the browser advertises WebMCP and the page already exposes the relevant tools. The current **도구 → AI 연결** page provides browser setup guidance and an AI usage guide, not a tool connection switch. If tools are absent, use browser controls. Discover actual names and schemas; expected names are `holodori_context`, `holodori_catalog`, and `holodori_analyze`, possibly with host-added suffixes. Read [Analysis requests](#analysis-requests) for this route only.

Use data saved in that browser. If required owned/growth data is missing, explain what is missing and guide the user to **사용자 데이터** or their calculator backup. Do not assume all cards are owned, migrate unrelated browser data automatically, ask for Node installation, or run the previous bundled plugin in cloud code execution.

## Calculate and compare

- Keep the user's requested song, difficulty, team, leader/outfit, and goal. If the request says "현재 편성", start from the visible conditions. Distinguish **예상 점수** (expected/average) from theoretical maximum. Default to owned cards and expected score when the goal is unspecified, while respecting the website's actual optimization goal.
- Read a completed baseline, change only the requested scenario, calculate again, and compare scores using the same remaining conditions. Growth changes can affect parameters and skills; do not estimate score changes from power alone. Delta = scenario minus baseline; relative change uses baseline as denominator.
- Browser route: record original conditions, use analysis-only inputs, verify each changed value before calculating, and restore changed conditions after comparison. Recalculate the restored baseline to verify the result. Inputs may persist automatically even without a save button.
- Tool route: start with `holodori_context`; resolve IDs through `holodori_catalog` rather than inventing them. Use `holodori_analyze` and `scenario`, with the first team as baseline. Require `status: complete` before reporting scores.
- Never edit **사용자 데이터** ownership/growth, board layouts, presets, or saved analysis records merely to model a scenario. Apply or save results only when the user requests it. On failure, restore any changed analysis inputs and state which result or restoration remains unverified.
- Reuse completed results that answer follow-up questions. Keep additional experiments focused on the user's question instead of automatically running every possible team or growth stage.

## Explain the result

Report the song/difficulty, team and relevant leader/outfit, expected score, maximum score, changed assumption, and delta. State whether each scenario was separately reoptimized; do not describe that as a comparison with the same special order/frequency allocation held fixed. Mention important overrides and any displayed score-calibration warning. **빈도 조합 상위** in **배치 분석** is ordered by maximum score: its first row is not necessarily the best average. An expected-score comparison of visible rows is limited to those rows, not all possible combinations.

The displayed probabilities assume all notes are PERFECT and vary only active-skill activation; report exact versus sampled probabilities as labeled by the current page/result. Search narrows candidates and does not prove global optimality. For site-tool multi-chart rankings, scores are optimized separately for each chart, not a shared fixed order. Do not invent event bonuses or unavailable charts.

Browser results appear on their calculation screen; explain available site-tool results from their returned data. The user's analysis request does not authorize sharing collection data with unrelated services or changing saved user data.


# Browser calculation workflow

Use the host's documented browser controls and current visible labels. The labels below describe the calculator; inspect the live page rather than assuming control coordinates, option IDs, or browser APIs. Keep the user's tab open.

## Find the appropriate screen

- **배치 분석**: score an existing five-card team, optimize special-skill order and frequency reductions, and compare individual card awakening scenarios.
- **조합 탐색**: search team candidates under the visible fixed-card and ownership conditions. Read the screen's stated optimization objective; ordinary search optimizes maximum score, not necessarily average score.
- **사용자 데이터**: inspect saved ownership, memory count, member upgrades, and growth when needed. Do not change these for hypothetical calculations.
- **도구 → AI 연결**: browser setup guidance and a link to the AI usage guide. Read the guide in its separate tab and operate the original calculator tab. Use the ordinary analysis screens for calculation; this page has no tool connection switch or result panel.

Use the visible navigation. Preserve the current origin and URL style: online Pages may use hash routes while other deployments use path routes.

## Baseline on 배치 분석

1. Read **분석 조건**. Record the song, difficulty, five selected cards with their slot numbers and awakenings, **리더**, **리더 의상**, frequency locks, **파란 노드 전체** checkboxes, generation overrides, and song-score override. Also note visible memory/member-upgrade context when explaining a comparison.
2. Cards are five-star cards from five different members. A leading **●** in a card option means owned. A member name alone may identify multiple variants; use the complete displayed card name. The selected awakening is an analysis assumption and may differ from saved ownership growth. **명함** means 0 awakening; maximum is 5. The page calculates parameters at maximum card level.
3. Use **최적 배치 분석** and observe progress. While **분석 중 …%** is displayed, do not interpret a previous result as the new calculation. A short browser wait timing out does not mean the calculation failed: read progress before retrying, and do not start duplicate jobs.
4. Verify completion with the ready calculation button, no running progress/error, and a populated **분석 결과** for the chosen song/difficulty and analysis power. If **입력 조건 또는 사용자 데이터가 변경되었습니다** is shown, the displayed result is stale; require a fresh calculation until the warning disappears. After a scenario change, require a new completed calculation even when its score happens to equal the baseline.
5. Read the concise result summary rather than the entire skill timeline: **예상 점수**, **최고점 · 밀어치기 적용**, **최고점 · 밀어치기 미적용**, **추천 스페셜 발동 순서**, and per-card **쿨타임 …% 감소**. Record the selected frequency row if relevant. A result can remain visible while inputs change: **분석 당시 종합력** belongs to the completed run; **자동 계산 종합력** belongs to current inputs.

## Growth comparison and restoration

For "현재 편성에서 코보를 명함으로 가정하면?" find Kobo's actual selected slot; do not assume a fixed slot across users. Record its original awakening and completed baseline. Use that slot's **N번 카드 개화 단계 낮추기/높이기** buttons until the adjacent numeric value is the requested stage, then read the updated input state before starting calculation. Hold every other condition constant. Read the newly completed scenario result, compute its difference from the baseline, restore the original stage, and calculate once more to verify restoration. Do not change Kobo's ownership record in **사용자 데이터**.

Each **최적 배치 분석** run optimizes special order/frequency allocations for that scenario. Report growth deltas as separately reoptimized outcomes. If the user asks to keep the exact order/allocation fixed, first verify that the available controls or tools support it; do not claim an optimized run holds them fixed.

Use the same approach for another card or outfit comparison. If changing a card alters dependent leader/outfit options, record and restore those too. On an interrupted or failed calculation, restore changed inputs when browser control remains available; otherwise tell the user which condition still needs restoration. Do not save/delete analysis records or presets during a comparison.

## Input meanings that affect conclusions

- **빈도 감소 고정** uses **미사용**, 0%, 4%, 8%, 12%. **미사용** leaves the reduction available for optimization; it does not fix it to 0%. These are optimization variables and are not unlocked solely by the saved blue-board state.
- **파란 노드 전체** is a hypothetical full-blue-board condition. Leave it off for the user's actual saved board unless that scenario was requested.
- A blank **기수 보너스 덮어쓰기 (0~900)** or **가창 보너스 덮어쓰기 (0~10%)** uses the actual board value. Entering 0 explicitly overrides it to zero. Preserve blanks versus numeric zeros.
- **리더 의상** affects team power; selecting a leader alone does not determine the full condition. Read the displayed costume effect.
- **최고점 · 밀어치기 적용** assumes relevant manually judged notes at a special-skill start boundary benefit from the new special effect. **밀어치기 미적용** excludes that boundary handling. Keep the same maximum-score definition when comparing runs.
- **예상 점수** incorporates active-skill randomness under the calculation's PERFECT-note assumption. It does not predict a player's actual accuracy.
- **빈도 조합 상위** is sorted by maximum score. The displayed average and success probabilities can favor another row. Select a row if its detailed result is needed, and distinguish "best among the displayed rows" from a global optimum.
- Probability labels and sample counts belong to the current page. Do not apply site-tool sample limits to screen calculations. Board/connection recommendations are separate operations; do not apply a recommended board layout as part of ordinary score comparison.

## Team search

On **조합 탐색**, record the visible search conditions before changing them. For owned-only requests, use **내가 보유한 카드만 사용하기**. Record fixed cards/awakenings, leader/outfit, candidate awakening, probability restrictions, overrides, and output count. The page explains how higher saved awakenings override lower automatic-candidate stages; read that guidance before treating every candidate as having the chosen stage.

Run **최적 조합 탐색** and wait through candidate screening, detailed analysis, and probability phases. Summarize the completed visible candidates and their assumptions. For a request to maximize average score, explain an available screen's maximum-score objective; use an advertised expected-score site tool if available, or compare a bounded set of candidates and state the search limitation. Restore search inputs changed for a temporary comparison.

## Continuing the conversation

Keep a compact baseline/scenario comparison in the conversation, including assumptions and values, so questions about existing results do not require another run. For a new experiment, read current page inputs again; the user may have changed them. If a requested growth scenario cannot be modeled by analysis inputs, explain that limitation instead of changing saved growth or making up a numerical result.


# Analysis requests

These illustrate request structure. Replace every angle-bracket placeholder with an ID returned by the current page's catalog; they are not executable examples. Use the live tool schema if it differs.

## Bounded search

```json
{
  "schemaVersion": 1,
  "operation": "search",
  "charts": [{"songId": "<song-id>", "difficulty": "easy"}],
  "objective": "expected",
  "ownedOnly": true,
  "search": {
    "fixedCardIds": ["<required-card-id>"],
    "leaderCharacterId": "<member-id>",
    "leaderOutfitCardId": "<owned-outfit-card-id>",
    "maxTeams": 4
  }
}
```

Omit candidateCardIds to use owned five-star cards. If more than 200 are eligible, use catalog queries to narrow them. Fixed cards must be included in an explicitly supplied candidate pool. The leader defaults to the first team member; specifying a leader and owned outfit makes the intended comparison clear.

## Compare awakening

```json
{
  "schemaVersion": 1,
  "operation": "compare",
  "charts": [{"songId": "<song-id>", "difficulty": "easy"}],
  "teams": [
    {"name": "현재", "cardIds": ["<c1>", "<c2>", "<c3>", "<c4>", "<c5>"], "leaderCharacterId": "<member-id>", "leaderOutfitCardId": "<outfit-card-id>"},
    {"name": "4개화 가정", "cardIds": ["<c1>", "<c2>", "<c3>", "<c4>", "<c5>"], "leaderCharacterId": "<member-id>", "leaderOutfitCardId": "<outfit-card-id>", "scenario": {"awakeningByCardId": {"<c1>": 4}}}
  ]
}
```

Each team needs five five-star cards from different members. Limits: three charts, eight teams, beamWidth ≤64, probabilitySamples ≤5000, one active job, 30 seconds. Returned assumptions and provenance explain the calculation's scope. `memberBoards` and `boardCards` scenarios replace complete mappings; do not fabricate node/socket IDs when catalog/context information is insufficient.
