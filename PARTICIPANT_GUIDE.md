# API Lifecycle Workshop — Participant Guide

## Build, Test & Ship APIs with Postman Agent Mode + MCP

Welcome! In the next 35 minutes, you'll go from an API spec to a fully tested, AI-orchestrated collection — and watch an AI agent use that API autonomously through MCP.

---

## Before We Start

Make sure you have:

- [ ] Postman installed ([desktop](https://www.postman.com/downloads/) or [web](https://www.postman.com))
- [ ] A Postman account (free is fine)

You'll need these links (also on screen):

| Resource | Link |
|----------|------|
| Fork the Documentation Collection | `{{FORK_LINK}}` |
| OpenAPI Spec (for import) | `[SPEC_URL](https://github.com/ialimustufa/api-workshop/blob/main/openapi.yaml)` |
| Source Code (optional) | `{{REPO_URL}}` |
| API Base URL | `https://events-api-production-80fe.up.railway.app` | 
| Postman Collection | `[LINK](https://studentconnects.postman.co/workspace/API-Day-Singapore~a643e53e-5d56-49b0-9d59-70dfc9f91540/overview)` |

---

## The API You're Working With

The **Event Scheduler API** lets you manage calendar events. It has:

- **Registration** — get your personal API key
- **CRUD** — create, read, update, delete events
- **Smart Scheduling** — find available time slots in your day
- **Progress Tracking** — the API tracks what you've done

Every participant gets their own isolated data. Your events are yours — nobody else can see them.

---

## Part 1 — Setup & Explore (12 min)

### Step 1: Fork the Documentation Collection

1. Click the **fork link** on screen (or from the table above)
2. Choose your workspace when prompted
3. Click **Fork Collection**

This gives you a reference collection with every endpoint documented and ready to use.

### Step 2: Register & Get Your API Key

1. In the forked collection, open **Register** → `POST /register`
2. Go to the **Body** tab. Fill in your details:
```json
{
  "name": "Your Name",
  "email": "your.email@example.com"
}
```
3. Click **Send**
4. Copy the `api_key` from the response

> **Registered twice by accident?** No problem — the API returns your existing key. It's designed for this.

### Step 3: Save Your API Key

1. Click on the collection name in the sidebar
2. Go to the **Variables** tab
3. Find the `api_key` variable
4. Paste your key in the **Current Value** column
5. **Save** (Ctrl/Cmd + S)

### Step 4: Verify It Works

1. Open **Get Events** → `GET /events`
2. Click **Send**
3. You should see:
```json
{
  "count": 0,
  "events": []
}
```

✅ If you got a `200` response with an empty array, you're in!

❌ If you got a `401`, check that your `api_key` variable is saved correctly.

### Step 5: Create Some Events

Create 3–4 events on the same day. Open **Create Event** → `POST /events` and modify the body:

```json
{
  "title": "Morning Standup",
  "description": "Daily team sync",
  "start_time": "2026-04-15T09:00:00Z",
  "end_time": "2026-04-15T09:30:00Z",
  "tag": "meeting"
}
```

Hit **Send**, then change the title, time, and tag to create more events. Try to fill up a day with varied events.

**Available tags:** `meeting`, `focus`, `break`, `social`, `general`

> **Try breaking it:** Create an event that overlaps with an existing one. You'll get a `409 Conflict` — that's the API protecting your schedule.

### Step 6: Find a Time Slot

Open **Find Slot** → `POST /events/find-slot`:

```json
{
  "date": "2026-04-15",
  "duration_minutes": 45,
  "preferred_time": "afternoon"
}
```

The API analyzes your schedule and returns available gaps. Preferred time options: `morning` (8am–12pm), `afternoon` (12pm–5pm), `evening` (5pm–9pm).

### Step 7: Import the OpenAPI Spec

1. In Postman, click **Import** (top left)
2. Paste the spec URL or upload the file (link on screen)
3. Click **Import**
4. You'll see a new API or collection appear — this is the raw spec

You now have two things: your **documentation collection** (the fork) and the **imported spec**. Next, Agent Mode takes over.

---

## Part 2 — Postman Agent Mode (15 min)

This is the main event. You'll use Agent Mode to build, orchestrate, and test — all from prompts.

### How to Open Agent Mode

In Postman, look for the **Agent Mode** option (sparkle/AI icon). Click it to open the AI assistant panel.

### Task 1: Create a Collection from Your Spec

Ask Agent Mode to read your imported spec and build a working collection:

> **Suggested prompt:**
> "Read my imported Event Scheduler API spec and create a new collection with organized folders and working requests for all endpoints. Use the `base_url` and `api_key` collection variables."

**What to watch for:**
- Agent Mode reads the spec and creates folders (Auth, Events, Scheduling, etc.)
- Each request gets proper headers, body templates, and variable references
- The `x-api-key` header uses `{{api_key}}` from your collection variables

**Once it's done:** Open any request in the generated collection and hit Send. It should work — Agent Mode wired up the auth and variables for you.

> **Didn't work?** Tell Agent Mode: "The request to [endpoint] is returning a [error code]. Can you check the request configuration and fix it?"

### Task 2: Orchestrate a Multi-Step Flow

Now ask Agent Mode to build a chained workflow — where one request's output feeds into the next:

> **Suggested prompt:**
> "Create a flow called 'Smart Booking' that: first calls find-slot to get an available 30-minute slot in the morning, then uses the first available slot to create a new event called 'Agent Booked Meeting'. Chain the response data from step 1 into step 2."

**What to watch for:**
- Agent Mode writes scripts that extract data from the find-slot response
- It sets variables like `slot_start_time` and `slot_end_time`
- The create-event request uses those variables in its body
- It handles the data flow between the two API calls

**Run the flow** and verify: did it find a slot and book an event? Check with `GET /events` to see the new event.

> **Want to try your own?** Here are some flow ideas:
> - "Book 3 back-to-back 30-minute meetings in the morning"
> - "Find a slot, book it, then verify it appears in the events list"
> - "Create an event, update its tag to 'focus', then list all focus events"

### Task 3: Generate Tests for Your Flow

Now lock it down with tests:

> **Suggested prompt:**
> "Add tests to my Smart Booking flow. For find-slot, verify it returns available_slots with start_time and end_time fields. For create event, verify it returns 201 and the event title matches 'Agent Booked Meeting'. Add a test that the event's time matches the slot we found."

**What to watch for:**
- Agent Mode adds test scripts to each request in the flow
- Tests check status codes, response schemas, and data relationships
- It may add edge case tests automatically

**Run the flow again** — look at the **Test Results** tab. Green checkmarks mean everything passed.

> **Want more?** Try:
> - "Add a negative test — create an overlapping event and verify the API returns 409"
> - "Add a test that verifies find-slot returns no results when the day is fully booked"
> - "Generate a full test suite for all CRUD operations"

> **Test failed?** Ask Agent Mode: "This test is failing with [error message]. Can you debug and fix it?"

---

## Part 3 — MCP: AI Agent Meets Your API (8 min)

The facilitator will demo this live on screen. Watch how:

1. **An MCP server is generated** from the API spec using Postman
2. **The API key is configured** in the MCP server (the agent authenticates as a specific user)
3. **An AI agent connects** and uses the API as tools:
   - It calls `list_events` to check the schedule
   - It calls `find_slot` to find availability
   - It calls `create_event` to book a new event
   - All through structured MCP tool calls — no copy-pasting, no manual requests

**Key takeaway:** The same API you used manually, then orchestrated with Agent Mode, is now being used autonomously by an AI agent. That's the full lifecycle.

### Replicate It Later (Self-Paced)

The MCP setup steps are documented in your forked collection. After the workshop:

1. Generate your MCP server from the collection/spec in Postman
2. Configure it with your API key
3. Connect it to Claude, ChatGPT, or any MCP-compatible AI agent
4. Ask the agent to manage your schedule

The API stays live for `{{DURATION}}` after the workshop. Your key still works.

---

## Part 4 — Check Your Progress (2 min)

### See Your Milestones

Hit `GET /progress` with your API key to see what you've accomplished:

| Milestone | What it means |
|-----------|---------------|
| ✅ Registered | You got your API key |
| ✅ First event created | You created at least one event |
| ✅ 3+ events same day | You built a real schedule |
| ✅ Used find-slot | You searched for availability |
| ✅ Updated an event | You modified an existing event |
| ✅ Deleted an event | You removed an event |

### Get Your Certificate

Open this in your browser:

```
{{BASE_URL}}/progress/certificate?api_key=YOUR_API_KEY
```

Replace `YOUR_API_KEY` with your actual key. Screenshot it, share it — you earned it!

---

## Quick Reference — API Endpoints

| Method | Endpoint | What it does |
|--------|----------|-------------|
| `POST` | `/register` | Register and get your API key |
| `POST` | `/events` | Create a new event |
| `GET` | `/events` | List your events (filters: `date`, `from`, `to`, `tag`) |
| `GET` | `/events/:id` | Get a single event |
| `PATCH` | `/events/:id` | Update an event |
| `DELETE` | `/events/:id` | Delete an event |
| `POST` | `/events/find-slot` | Find available time slots |
| `GET` | `/progress` | Check your workshop milestones |
| `GET` | `/leaderboard` | See everyone's progress |
| `GET` | `/progress/certificate` | Your shareable achievement page |
| `GET` | `/health` | API health check |

### Authentication

All endpoints (except `/register`, `/health`, `/leaderboard`) require:

```
Header: x-api-key: your_api_key_here
```

In Postman, this is handled automatically via the `{{api_key}}` collection variable.

### Event Tags

`meeting` · `focus` · `break` · `social` · `general`

### Find-Slot Preferred Times

`morning` (8am–12pm) · `afternoon` (12pm–5pm) · `evening` (5pm–9pm)

---

## Troubleshooting

**"401 Unauthorized"**
→ Your API key is missing or wrong. Check: collection variables → `api_key` → is the current value set? Did you save?

**"409 Conflict" on event creation**
→ Your new event overlaps with an existing one. The response tells you which event conflicts. Change the time and retry.

**"400 Bad Request"**
→ Something's wrong with your request body. Common causes: missing `title`, `end_time` before `start_time`, past dates. Read the error message — it tells you exactly what's wrong.

**Agent Mode isn't responding**
→ Try a shorter, more specific prompt. Instead of a long paragraph, start with one task: "Generate tests for the Create Event endpoint."

**Agent Mode generated a broken request**
→ Tell it: "This request returns [status code]. Here's the error: [paste error]. Fix it." Agent Mode can debug its own output.

**My collection variables aren't working**
→ Make sure you're editing the **Current Value** column (not Initial Value). Save after editing (Ctrl/Cmd + S).

**Can't find the import button**
→ Top-left corner of Postman → **Import** button. You can paste a URL or drag-drop a file.

---

## After the Workshop

- **The API stays live** for `{{DURATION}}` — keep experimenting
- **The code is public** — clone the repo and run it locally: `{{REPO_URL}}`
- **Your MCP setup** — follow the steps in the documentation collection to connect an AI agent
- **Postman learning resources** — `{{POSTMAN_RESOURCES_LINK}}`

Thanks for building with us! 🛠️
