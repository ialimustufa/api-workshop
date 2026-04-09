# API Lifecycle Workshop — Trainer Guide

## Facilitator Handbook: Everything You Need to Run This Workshop

---

## Workshop Overview

| Detail | Value |
|--------|-------|
| Total session | 50 minutes |
| Active content | 35 minutes |
| Float/buffer | 15 minutes |
| Participants | Up to 40 |
| TAs | 2 |
| Participant level | Comfortable with APIs; Postman experience helpful but not required |

### Learning Objectives
By the end, participants will have:
1. Prototyped an API by creating, testing, and exploring endpoints hands-on
2. Used Postman Agent Mode to generate a collection from a spec
3. Used Agent Mode to orchestrate a multi-step API flow with chained data
4. Used Agent Mode to generate and run meaningful tests
5. Seen how an MCP server turns an API into tools an AI agent can use
6. Understood the full API lifecycle: spec → collection → flow → tests → MCP → agent

---

## One Week Before

- [ ] Deploy the API server and verify all endpoints work
- [ ] Generate the OpenAPI spec and host it at a stable URL
- [ ] Create the documentation collection in Postman and generate a fork link
- [ ] Test the fork link from an incognito/logged-out browser
- [ ] Set up the public GitHub repo with clean README
- [ ] Prepare your MCP demo environment (server generated, AI agent connected, your API key configured)
- [ ] Run through the entire workshop flow yourself, end to end, timing each act
- [ ] Brief TAs — walk them through the flow, show them common failure points
- [ ] Have TAs run through the full flow independently
- [ ] Prepare the pre-workshop slide/screen with all links
- [ ] Set placeholder values in the participant guide (`{{FORK_LINK}}`, `{{SPEC_URL}}`, etc.) and replace with real URLs
- [ ] Distribute the participant guide to registered attendees (or plan to share at session start)

---

## Day Of — Pre-Workshop Setup (30 min before)

- [ ] Hit `GET /health` — confirm server is up
- [ ] Run `POST /admin/reset` with your admin key — clean slate
- [ ] Register yourself: `POST /register` — get your facilitator API key
- [ ] Create 4–5 demo events on a specific day (you'll use these for live demos)
- [ ] Verify the fork link works right now
- [ ] Verify the spec import works right now
- [ ] Open your MCP demo environment — verify the AI agent can reach the API
- [ ] Test the certificate page in a browser: `/progress/certificate?api_key=YOUR_KEY`
- [ ] Test the leaderboard: `GET /leaderboard` — should show just your name
- [ ] Set up your screen: Postman open, browser tab ready for certificate/leaderboard, links slide ready
- [ ] Brief TAs on their positions (see TA section below)
- [ ] Confirm WiFi/network can handle 40 concurrent Postman users

### Your Screen Layout
- **Primary:** Postman (you'll demo everything here)
- **Tab 1:** Browser with the leaderboard URL (for Act 4)
- **Tab 2:** Browser with your certificate URL (for Act 4)
- **Tab 3:** MCP-connected AI agent (for Act 3 demo)
- **Slide/overlay:** Links for participants (fork, spec, repo, base URL)

---

## Minute-by-Minute Facilitator Script

---

### ACT 1 — SETUP & EXPLORE (0:00–12:00)

---

#### 0:00–2:00 | Scene Setting

**Mode:** You talk, they listen
**On screen:** Slide with links

**SAY:**
> "Welcome to the API Lifecycle Workshop. In the next 35 minutes, you're going to experience the full lifecycle of an API — from an OpenAPI spec, to a fully tested and orchestrated collection, to an AI agent using that API autonomously."
>
> "The star of today is Postman Agent Mode. It's going to build your collection, chain your requests into workflows, and write your tests. You'll direct it with prompts — think of it as pair programming with AI for your API workflow."
>
> "The API you're working with is the Event Scheduler — a live, hosted API for managing calendar events. The code is open source [gesture to link], it's running right now, and each of you will get your own API key with your own isolated data."
>
> "Let's start. You should see four links on screen..."

**SHOW:** Point to each link — fork, spec, repo, base URL

**TA CUE:** TAs stand at edges of the room, ready to circulate

---

#### 2:00–4:00 | Fork & Register

**Mode:** You demo on screen, they follow along

**SAY:**
> "First, fork the documentation collection. Click the fork link — it'll ask you to pick a workspace. Any workspace is fine. Fork it."

**WAIT** 30 seconds. Let them click.

> "Now open the Register request in your forked collection. Put in your name and email in the body. Hit Send."

**DEMO on screen:** Show yourself sending the register request. Highlight the `api_key` in the response.

> "See that `api_key`? Copy it. Now go to your collection variables — click the collection name, then the Variables tab. Paste your key into the `api_key` current value. Save."

**DEMO on screen:** Show the variable being set and saved.

**TA CUE:** Both TAs circulate now. Common issues: participant doesn't have a Postman account, fork link opens wrong, can't find Variables tab.

---

#### 4:00–6:00 | First Call & Verification

**SAY:**
> "Let's verify it works. Open the Get Events request and hit Send."

**DEMO on screen:** Show the 200 response with empty array.

> "200 OK, empty events array. That means your key works and your schedule is empty. If you're seeing a 401, your API key variable isn't saved — raise your hand for a TA."

**CHECKPOINT:** Quick scan of the room.

> "Hands up if you have NOT gotten a 200 response yet."

**TA CUE:** TAs go to raised hands immediately.

**TIMING DECISION:** If more than ~8 people are stuck, spend 1–2 minutes of float here. If it's 3 or fewer, TAs handle while you continue.

---

#### 6:00–10:00 | Build a Schedule

**SAY:**
> "Now let's fill your schedule. Open Create Event and make an event."

**DEMO on screen:** Create one event live. Show the request body, hit send, show the 201 response.

> "Your turn. Create 3 to 4 events on the same day. Use different times, different tags. Tags are: meeting, focus, break, social, general. Make it your real schedule or make it up — doesn't matter, just fill the day."

**LET THEM WORK** for 2 minutes. Don't narrate. Let TAs handle questions.

> "Now try to break it. Create an event that overlaps with one you already made."

**DEMO on screen** (optional): Show the 409 conflict response. Point out the `conflicting_event` in the response.

> "409 Conflict — the API tells you exactly which event is in the way. Try a missing title or a past date too — see what errors you get."

**LET THEM WORK** for 1 minute.

---

#### 10:00–11:00 | Update, Delete, Find Slot

**SAY:**
> "Quick round: update one event — change its title or time. Delete one you don't need. Then hit Find Slot — ask for a 45-minute gap in the afternoon."

**DEMO on screen:** Show find-slot request and response with available gaps.

> "The API analyzed your schedule and found the openings. This is the smart endpoint — and it'll be important when we orchestrate flows in a moment."

---

#### 11:00–12:00 | Import the Spec

**SAY:**
> "Last setup step. Click Import in Postman — top left. Paste the spec URL [point to screen] or upload the file. Hit Import."

**DEMO on screen:** Show the import flow.

> "You now have two things: the documentation collection you forked, which is your reference, and the imported spec, which is the machine-readable definition. Agent Mode is going to work with that spec next."

**CHECKPOINT:**
> "Everyone should have: a forked collection with your API key set, a few events in your schedule, and the spec imported. Thumbs up if you're ready to move on."

**TIMING CHECK:** You should be at roughly 12:00. If you're at 14:00, that's fine — you have float. If you're at 16:00+, compress Act 2 Task 1 by doing it as a demo rather than having everyone replicate.

---

### ACT 2 — AGENT MODE (12:00–27:00)

---

**TRANSITION SAY:**
> "Everything you just did — forking, setting up variables, creating requests — Agent Mode can do that from a spec. Let's see it."

---

#### 12:00–17:00 | Task 1: Collection from Spec

**YOU DEMO (12:00–13:30):**

**SAY:**
> "I'm going to ask Agent Mode to read the spec and build me a working collection."

**Open Agent Mode** on screen. Type the prompt (or copy from your notes):

> *"Read my imported Event Scheduler API spec and create a new collection with organized folders and working requests for all endpoints. Use the base_url and api_key collection variables."*

**NARRATE as it works:**
> "Watch — it's reading the spec... it's creating folders... see how it's setting up the auth header with the api_key variable? It's not just copying endpoints, it's building a structured, usable collection."

When it finishes:

> "Let me test one." Open a request from the generated collection. Send it. Show the response.

> "It works. Agent Mode built a complete, functional collection from a spec in about 30 seconds."

**THEY DO (13:30–17:00):**

> "Your turn. Open Agent Mode and prompt it — you can use the same prompt or put it in your own words. The prompt is in your participant guide."

**LET THEM WORK.** This is the first "wow" moment. Give them space.

**CIRCULATE or narrate from the front:**
> "Once your collection is generated, try sending a request from it. Does it work? If not, tell Agent Mode what went wrong — it can fix its own output."

**TA CUE:** TA 2 focuses on Agent Mode issues — prompts that don't work, generated collections with wrong variable names.

---

#### 17:00–22:00 | Task 2: Orchestrate a Flow

**YOU DEMO (17:00–18:30):**

**SAY:**
> "Individual requests are one thing. But real API work is flows — where step one feeds into step two. Let's build one."

**Type the prompt:**

> *"Create a flow called 'Smart Booking' that: first calls find-slot to get an available 30-minute slot in the morning, then uses the first available slot from the response to create a new event called 'Agent Booked Meeting'. Chain the response data from step 1 into step 2."*

**NARRATE as it works:**
> "It's creating the find-slot request... now it's writing a script to extract the first available slot from the response... see that? It's setting variables for start_time and end_time... now it's building the create event request using those variables."

When it finishes, **run the flow** on screen. Show the find-slot response, then the created event.

> "It found a slot, extracted the times, and booked a meeting in that slot. Two API calls, chained automatically."

**THEY DO (18:30–22:00):**

> "Build your own flow. You can use the same prompt, or try something different."

**Share ideas verbally:**
> "Some ideas: book three back-to-back meetings in the morning. Or: find a slot, book it, then verify it shows up in your events list. Or make up your own — Agent Mode can handle complex chains."

**KEY COACHING POINT:**
> "If the first output isn't perfect, iterate. Tell Agent Mode what to change. 'The start time is wrong — use the first slot's start_time, not the end_time.' Refinement is part of the process."

**LET THEM WORK.** This is creative time. Don't rush.

**TA CUE:** Both TAs active. Common issues: variable chaining not working (Agent Mode used wrong variable name), flow runs out of order.

---

#### 22:00–27:00 | Task 3: Generate Tests

**YOU DEMO (22:00–23:30):**

**SAY:**
> "You've got a working flow. Now let's make sure it stays working."

**Type the prompt:**

> *"Add tests to my Smart Booking flow. For the find-slot step, verify the response has available_slots and each slot has start_time and end_time. For the create event step, verify it returns 201 and the event title matches 'Agent Booked Meeting'. Add a test that the created event's time matches the slot we found."*

**NARRATE as it works:**
> "It's adding test scripts to each request... see how it's not just checking status codes? It's validating the schema, the data relationships between the two steps."

**Run the flow.** Show the test results tab with green checkmarks.

> "All passing. Agent Mode wrote tests that verify not just that each request succeeded, but that the data flowed correctly between them."

**THEY DO (23:30–27:00):**

> "Add tests to your flow. Start with the prompt in your guide, then push further."

**Share ideas verbally:**
> "Try: 'Add a negative test — create an overlapping event and verify the 409.' Or: 'Test what happens when find-slot returns no available slots.' Or just: 'Generate a complete test suite for all my CRUD operations.'"

> "If a test fails, don't fix it manually — paste the error back to Agent Mode and ask it to debug. It can fix its own tests."

**LET THEM WORK.**

**CHECKPOINT at 27:00:**
> "Run your flow with tests one more time. Those green checkmarks? That's an AI-built, AI-tested, orchestrated API workflow. You didn't write a single test script by hand."

**TIMING CHECK:** You should be at 27:00. If you're at 30:00, that's fine — you used some float. Compress Act 3 slightly. If you're at 24:00, give them 3 more minutes here — this is where the deep learning happens.

---

### ACT 3 — MCP: AI AGENT MEETS YOUR API (27:00–35:00)

---

**TRANSITION SAY:**
> "You've used AI to build and test your API workflow. Now let's flip it. Instead of you telling AI what to do with the API... what if AI could use the API on its own?"

---

#### 27:00–28:00 | What is MCP

**SAY:**
> "MCP — Model Context Protocol — is how AI agents talk to APIs. Instead of an agent guessing how your API works from documentation, MCP gives it a structured toolbox: 'here are the tools you can use, here are their parameters, here's how to authenticate.' It's the bridge between an LLM and your API."

> "We're going to turn the Event Scheduler into an MCP server and connect an AI agent to it."

---

#### 28:00–31:00 | Build MCP Server (You Drive)

**SAY:**
> "Watch my screen. I'm going to generate an MCP server from this API using Postman."

**DEMO on screen:** Walk through the MCP server generation in Postman step by step. Go at a pace where they can follow conceptually but don't expect them to replicate in real time.

**NARRATE the key moments:**

When you see the tool definitions:
> "See these tools? `create_event`, `list_events`, `find_slot`. Each one maps to an API endpoint. The agent sees these as actions it can take."

When you configure the API key:
> "Here's where the API key goes. The agent authenticates as a specific user — my key, my data. When it creates an event, it creates it on MY schedule, not anyone else's."

When the server is running:
> "MCP server is up. It's listening for tool calls from an AI agent."

---

#### 31:00–34:00 | Live AI Agent Demo

**SAY:**
> "I've connected an AI agent to this MCP server. Let's see what happens."

**Switch to your AI agent tab.**

**Prompt 1:**
> "What's on my schedule for April 15th?"

**NARRATE:** "It's calling `list_events`... and here are my events. The same data I see through the API, the agent sees through MCP."

**Prompt 2:**
> "Find me a free 45-minute slot in the afternoon and book a focus block called 'Deep Work'."

**NARRATE:** "Watch — it's calling `find_slot` first... found availability... now it's calling `create_event` with the slot times... and it's booked."

**Prompt 3:**
> "Confirm it's on my schedule now."

**NARRATE:** "It calls `list_events` again... and there it is. The agent just did what you did manually in Act 1, and what your orchestrated flow did in Act 2. Same API, three different interfaces."

**THE MOMENT:**

> "Spec. Collection. Flow. Tests. MCP. Agent. That's the API lifecycle — and AI was involved at every step."

**Pause.** Let it land.

---

#### 34:00–35:00 | Self-Paced Handoff

**SAY:**
> "The MCP setup steps are documented in your forked collection. The API stays live — your key still works. After this session, you can generate your own MCP server and connect it to Claude, ChatGPT, or any MCP-compatible agent."

> "This is a take-home exercise. The docs walk you through it step by step."

---

### ACT 4 — PROGRESS & CELEBRATION (35:00–37:00)

---

#### 35:00–36:00 | Check Progress

**SAY:**
> "Let's see how you did. Hit `GET /progress` in your collection — or from your generated collection, either works."

Give them 15 seconds.

> "How many milestones did you hit? Six out of six? Five?"

**Switch to leaderboard tab.** Hit refresh.

> "Here's the leaderboard."

**Read out the top 3 names.** Energy, not ceremony.

> "[Name] — all six! [Name] — five. [Name] — five. Great work."

---

#### 36:00–37:00 | Certificate & Close

**SAY:**
> "Last thing. Open this URL in your browser..."

**Show on screen:** `{{BASE_URL}}/progress/certificate?api_key=YOUR_KEY`

> "Replace YOUR_KEY with your actual API key. This is your achievement page — screenshot it, share it, post it."

**Wait 15 seconds** for people to open it.

> "Here's what happened today. You started with a spec. Agent Mode turned it into a collection, orchestrated a multi-step flow, and wrote the tests. Then an AI agent used that same API through MCP — autonomously. Spec to agent in 35 minutes. That's the API lifecycle."

**Share links:**
> "The repo is public [point]. The API stays live. The participant guide has everything you need to keep going."

> "Thank you for building with us. Go ship something."

---

## TA Coordination

### TA 1 — "Setup & Collection Specialist"

| Act | Priority | Focus |
|-----|----------|-------|
| Act 1 (0:00–12:00) | **PRIMARY** | Fork issues, API key setup, variable saving, spec import |
| Act 2 (12:00–27:00) | Secondary | Help participants whose Agent Mode-generated collections have broken variables or auth |
| Act 3 (27:00–35:00) | Standby | Help anyone still catching up from earlier acts |
| Act 4 (35:00–37:00) | Active | Help with certificate URLs |

**Common issues you'll handle:**
- "I can't find the fork link" → show them on screen or send DM
- "401 error" → check their collection variables, make sure Current Value is set and saved
- "Import isn't working" → make sure they're clicking Import (not Open), try file upload instead of URL
- "I registered twice" → that's fine, same key returns. Reassure them.

### TA 2 — "Agent Mode & Flow Specialist"

| Act | Priority | Focus |
|-----|----------|-------|
| Act 1 (0:00–12:00) | Secondary | General support |
| Act 2 (12:00–27:00) | **PRIMARY** | Agent Mode prompt issues, broken flows, variable chaining, test failures |
| Act 3 (27:00–35:00) | Active | Help participants who want to start MCP setup |
| Act 4 (35:00–37:00) | Active | Help with certificate URLs |

**Common issues you'll handle:**
- "Agent Mode isn't doing anything" → check they have Agent Mode enabled, try a simpler prompt first
- "The generated collection doesn't work" → look at the auth header and variables, often a naming mismatch
- "My flow runs but the chaining is wrong" → look at the pre-request/post-response scripts, check variable names
- "Tests are failing" → read the error message, often it's a response structure mismatch. Have them ask Agent Mode to debug.
- "Agent Mode is slow" → try shorter prompts, break complex tasks into smaller ones

### Both TAs — General Rules

1. **Pre-register** with your own API key before the session. Run through the full flow once.
2. **Don't solve problems by doing it for them** — guide them to the fix. Except in Act 1 setup — there, speed matters.
3. **Watch for the "quiet stuck" person** — not everyone raises their hand. Walk the room and glance at screens.
4. **If you see a common issue affecting 5+ people**, alert the facilitator. It might warrant a 30-second group announcement.
5. **Keep a running count** of how many people are "caught up" at each checkpoint. Signal to facilitator: thumbs up (>80% ready) or flat hand (50-80%) or thumbs down (<50%).
6. **After Act 1**, anyone hopelessly behind can use the documentation collection directly — it already has working requests. They skip the Agent Mode collection generation but can still do flows and tests.

---

## Timing Signals Between Facilitator and TAs

Use simple hand signals so you don't interrupt the flow:

| Signal | Meaning |
|--------|---------|
| TA holds up 1 finger | "1 person still stuck, keep going" |
| TA holds up 5 fingers | "5+ people stuck, slow down" |
| TA thumbs up | "Everyone's good, move on" |
| TA flat hand wave | "Need 1 more minute" |
| Facilitator taps wrist | "Wrapping this section in 60 seconds" |

---

## Emergency Protocols — Detailed

### Server Goes Down

**Symptoms:** All participants get connection errors simultaneously.

**Response:**
1. TA 1 checks server status and restarts (should have SSH/deployment access)
2. Facilitator says: "We've got a server hiccup — one of the perks of live demos. While it comes back, let me show you something about how Agent Mode works under the hood..."
3. Fill with conceptual explanation or Q&A for 2–3 minutes
4. SQLite WAL mode means all data survives the restart. No one loses anything.
5. Once back: "We're live again. Pick up where you left off."

### Agent Mode Slow or Unresponsive (Multiple Participants)

**Symptoms:** 10+ people report Agent Mode spinning or not responding.

**Response:**
1. Facilitator: "Agent Mode is getting a lot of love right now. If it's slow, try a shorter, more specific prompt — one task at a time instead of three."
2. If it persists for Task 1: Facilitator demos the full collection generation on screen. Participants follow along visually and move to Task 2 when Agent Mode recovers.
3. If it persists for Task 2: Facilitator demos the orchestration. Participants try Task 3 (testing) which tends to be lighter.
4. Nuclear option: "Let me show you what Agent Mode generated for me earlier." Show pre-built output. Walk through it. The learning still happens.

### MCP Demo Fails Live

**Symptoms:** AI agent can't reach MCP server, or tool calls fail.

**Response:**
1. Have backup screenshots or a 60-second screen recording of a successful MCP demo
2. Walk through it: "Here's what happens when I ask the agent to book an event..." Show the sequence of tool calls and responses
3. The conceptual understanding is the objective — watching you explain a recording still achieves this
4. "The docs in your collection walk through the setup. You'll get this working on your own — the API stays live."

### One Participant Is Way Behind

**Response:**
1. TA directs them to the documentation collection (the fork). It already has working requests.
2. "Use this to catch up — create a few events, try find-slot, then jump back into wherever the group is."
3. They can always replicate Agent Mode steps after the workshop since the API stays live.
4. Don't let one person slow the group. TAs handle individuals; facilitator serves the room.

---

## Post-Workshop

- [ ] Save the leaderboard response (screenshot or JSON) for records
- [ ] Note any issues that came up — add to the pre-workshop checklist for next time
- [ ] Don't reset the database immediately — participants may want to grab their certificates after the session
- [ ] Reset the database before the next workshop session: `POST /admin/reset`
- [ ] Debrief with TAs: what worked, what was confusing, where did people get stuck?
- [ ] Update the participant guide if any steps were unclear
