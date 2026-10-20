# Bridge Context Space: context sharing in Microsoft Teams

> Canonical brief: [`../skill.md`](../skill.md). This is a small, first slice of Bridge, written for designers and PMs first. Developer notes are at the end.
>
> Flow board (FigJam): [Bridge Context Space - Teams flow](https://www.figma.com/board/9wPgZtsz3ynaYvXX8AcO8M)

## The idea in one paragraph

Every project gets one **Context Space** inside Microsoft Teams: a group chat for small teams, a channel for big ones. All files, chats, call transcripts and Figma links for that project live there. A Bridge agent sits in the space, keeps a set of plain project pages (a "wiki") up to date, and answers anyone's question with links to where the answer came from. When it doesn't know, it tells you who does and asks them for you. The aim is **context sharing instead of context switching**: nobody has to dig through five tools or interrupt five people to get up to speed.

How this maps to Bridge:

| Bridge idea | In the Teams Context Space |
|---|---|
| Map | The project pages: brief, people, decisions, open questions |
| Sources stay visible | Every answer links to the file, message, call moment or Figma frame |
| Change gate (no silent rewrites) | Decisions, scope and deadlines change only after the PM or owner approves |
| Visibility tiers | Bridge only uses what the person asking is allowed to see. A private DM with Bridge stays private |
| Context Work | Open questions are routed to an owner, and their reply becomes context |

## The three jobs

### 1. Create a project (about 5 minutes)

Started by the PM typing `@Bridge new project`, or by Bridge offering when it sees a new channel.

**Card 1: Project basics (PM)**
- Project name and a one-line goal
- Client or internal team
- Start date, end date, key milestones
- Big or small team? (small means group chat, big means channel)

Bridge then sets up the space: the chat or channel, a project folder in SharePoint, a Bridge tab, and a pinned brief.

**Card 2: People and roles (PM)**
- Add members and pick a role for each: PM, developer, visual designer, design strategist, team lead (others can be added).

Bridge then sends each person a short DM with three questions:
1. What do you own on this project?
2. What are you working on right now?
3. Where does your work live? (Figma file, repo, document)

These answers are how Bridge later knows **who to ask**.

**Card 3: Connect sources (anyone)**
- Figma file links
- Jira or Planner board
- GitHub repo (only if the project has code)
- Any existing SharePoint or OneDrive folder

Bridge writes the first project pages and posts a welcome message with starter questions people can try.

**Project folder layout** (created for every project, so all spaces look the same):

```
Project name/
  01 Brief/          goal, client, dates, scope
  02 Sources/        files people drop in
  03 Meetings/       call transcripts and meeting notes
  04 Decisions/      approved decisions, one page each
  05 Wiki/           pages Bridge keeps up to date (see below)
```

### 2. Keep it up to date (runs by itself)

| What happens | What Bridge does |
|---|---|
| A file is added or edited | Writes a short summary and a log entry |
| People chat in the channel | Picks up updates, questions and decisions |
| A call ends with a transcript | Writes meeting notes: decisions, actions, owners |
| A Figma file or comment changes | Notes what changed and links the frame |
| Something changes a **decision, scope or deadline** | Sends the PM or owner an approval card first. Nothing changes until they approve |
| Every night | Double-checks for anything it missed |
| Every week | Checks the pages for conflicts, old information and unanswered questions, and sends the PM a summary |

### 3. Ask in Teams

Ask in the project channel (`@Bridge ...`) or privately in a 1:1 chat with Bridge. Examples by role:

| Who | Asks | Gets |
|---|---|---|
| New joiner | "Catch me up on this project" | Onboarding brief: goal, who's who, decisions so far, links |
| PM | "What happened this week?" | Progress by person, blockers, decisions waiting for approval |
| Developer | "What's the final spacing on the card?" | Answer plus a link to the exact Figma frame |
| Visual designer | "What did the client say about the colours?" | Quote from the call, with the time it was said |
| Anyone | "Why did we drop dark mode?" | If not found: "Rahul owns visual design. Ask him?" with a button |

Every answer shows its sources. If Bridge is unsure, it says so and routes the question rather than guessing.

## What the project pages look like (the "LLM Wiki")

The **LLM Wiki** is an idea shared by Andrej Karpathy: instead of making the AI search raw files every time, the AI keeps a small folder of plain pages up to date as new material comes in, and answers from those pages. It has three parts:

1. **Raw sources**: the files, transcripts and messages, which are never edited.
2. **Wiki pages**: plain text (markdown) pages the AI writes and keeps current.
3. **A rules page**: tells the AI how pages are organised and what it may change.

For a project, the pages would be:

| Page | Holds |
|---|---|
| `index` | One-screen overview and a list of all pages |
| `brief` | Goal, client, dates, scope |
| `people` | Who owns what, what they're working on, where their work lives |
| `decisions` | Approved decisions with date, owner and source |
| `open-questions` | Questions not answered yet, and who they're waiting on |
| `meetings/<date>` | Notes from each call |
| `log` | A dated list of everything Bridge added |

### Do we need GitHub?

**No.** Karpathy shared the idea as a GitHub "gist", but that is only where the idea was posted. The LLM Wiki is just a folder of text pages, and it works anywhere files can be stored. For us, the project's **SharePoint folder** is the better home:

- It's where Teams already keeps channel files.
- It already has version history, so every change Bridge makes can be seen and undone.
- It already has the same permissions as the Teams channel, so nobody gets access they shouldn't have.

GitHub is only useful as **one more source** for projects that have code (Bridge reads the repo, issues and pull requests). It's optional.

## Three ways to build the agent

| Option | What it is | Good | Not so good |
|---|---|---|---|
| **A. Microsoft's own Channel Agent** | Microsoft's assistant built into Teams channels, plus "Facilitator" for meeting notes | Nothing to build. Worth testing to compare | Needs Microsoft 365 Copilot licences. Still in preview. Runs on Microsoft's AI, so we can't use Claude or our own page structure |
| **B. Copilot Studio agent using Claude** | Microsoft's low-code agent builder, which can now use Claude models | Little code. Lives in Teams | Needs Copilot Studio licensing. Hard to build the self-updating pages and approval cards |
| **C. Our own Bridge app in Teams, using the Claude API** | A Teams chat app that a developer builds, calling Claude | Full control over setup cards, project pages, approvals and "who to ask". Becomes part of Bridge | Needs a developer and some Azure hosting |

**Recommendation: C, after a quick no-build test (Step 0).** It's the only option that gives us Bridge's rules (approval before changes, sources on every answer, who-to-ask) and lets us use Claude.

Also worth knowing: news reports in mid-2026 said Anthropic plans its own Claude agent for Teams (like Claude in Slack). It isn't confirmed or released yet. If it ships, it could replace part of option C.

## What we need, step by step

### Step 0: Try it now, nothing to build

1. Pick one live project. Put all its files in one SharePoint folder with the layout above.
2. Turn on transcription for that project's calls.
3. Ask IT to switch on **Claude's Microsoft 365 connector**, which lets Claude (the chat app we already pay for) search SharePoint, OneDrive, Outlook and Teams, including meeting transcripts.
4. For two weeks, ask Claude the kinds of questions in the table above. Note which answers are good and which aren't.

This tells us whether the folder setup and transcripts are good enough **before** anyone builds anything.

### Step 1: Pilot Bridge in Teams on one project

- Bridge app in Teams, with the three setup cards
- Project pages that update themselves, with approval cards for decisions
- Answers with sources and "who to ask"

### Step 2: Roll out

- Every new project starts with `@Bridge new project`
- Figma, Jira and GitHub links
- Weekly summaries for PMs

### Shopping list

| We already have | We need to get |
|---|---|
| Microsoft Teams, SharePoint, OneDrive | **IT approval** for Bridge to read project chats, files and call transcripts (admin consent) |
| Teams call transcription | **Transcription on** by default for project calls |
| Microsoft 365 admin | **Azure space** to run Bridge (usually covered by the company's Microsoft agreement) |
| A Claude subscription | A **Claude API account** (Anthropic Console). The Claude chat subscription **does not** include API access; the API is billed separately by usage |
| Figma | A **Figma access token** so Bridge can read files and comments |
| | **A developer** for the pilot |

### About Figma: MCP or API?

- **Figma MCP** is how Claude reads a Figma file when a *person* is using Claude (desktop app, Claude Code). It's great for individuals.
- A Teams bot runs on its own with no person signed in to Figma, so it should use **Figma's regular API** with an access token. It gives the same file, frame and comment data.

## Questions to settle with the team

1. Which project should be the pilot?
2. Who approves changes on each project: always the PM, or the owner of that area?
3. What's the size cut-off between "group chat" and "channel"? (A starting suggestion: up to 5 people means a group chat.)
4. Should clients or external people ever see Bridge answers? (Suggested: no, at first.)
5. How long should transcripts and pages be kept after a project ends?

---

## Developer notes (for whoever builds Step 1)

Not needed to understand the flow; here so the developer has a starting point. Verify each item against current Microsoft and Anthropic docs, because these products change often.

- **Teams app**: a custom engine agent built with the Microsoft 365 Agents SDK, registered with Azure Bot Service, plus a Teams tab. Adaptive Cards for the setup cards and approvals.
- **Claude**: Messages API with tool use (search pages, open transcript, read Figma, find owner) and citations. Cache the project's `index` and `people` pages with prompt caching. Use a smaller model for summarising changes and a larger one for answers.
- **Microsoft Graph**: create channels and folders; change notifications for files, channel messages and call transcripts; a nightly reconciliation pass, because notifications can be missed.
- **Transcripts**: org-wide `OnlineMeetingTranscript.Read.All` (admin consent). The resource-specific (per-meeting) transcript permission covers scheduled chat meetings, **not channel meetings**, per Microsoft's docs. Channel meeting recordings live in the channel's SharePoint site.
- **Messages**: resource-specific consent per team or chat (`ChannelMessage.Read.Group`, `ChatMessage.Read.Chat`) keeps access scoped to projects that installed Bridge.
- **Storage**: wiki pages as markdown in the project's SharePoint library (keeps version history and Teams permissions). A small registry table maps chat or channel ID to project folder. Add a search index only when pages outgrow the model's context.
- **Permissions**: always answer as the asking user, using only content they can access.

## Sources

- [Claude's Microsoft 365 connector (Anthropic support)](https://support.claude.com/en/articles/12542951-enabling-and-using-the-microsoft-365-connector)
- [Claude Microsoft 365 connector docs](https://www.claude.com/docs/connectors/microsoft/365)
- [Claude models in Microsoft 365 Copilot and Copilot Studio](https://www.itechguides.com/anthropics-claude-models-are-now-available-in-microsoft-365-copilot-what-that-means-in-2026/)
- [Report: Anthropic plans a Claude agent for Teams (WinBuzzer, July 2026)](https://winbuzzer.com/2026/07/01/anthropic-eyes-claude-agent-for-microsoft-teams-xcxwbn/)
- [Agents in Microsoft Teams FAQ (Channel Agent, Facilitator)](https://support.microsoft.com/office/f7317b78-fd53-4cfe-88f0-f0a0751a4150)
- [Custom engine agents overview (Microsoft Learn)](https://learn.microsoft.com/microsoft-365/copilot/extensibility/agents-overview)
- [Teams meeting transcripts API overview (Microsoft Learn)](https://learn.microsoft.com/en-us/microsoftteams/platform/graph-api/meeting-transcripts/overview-transcripts)
- [Transcript and recording change notifications (Microsoft Learn)](https://learn.microsoft.com/en-us/graph/teams-changenotifications-callrecording-and-calltranscript)
- [Karpathy's LLM Wiki pattern, explained](https://blog.starmorph.com/blog/karpathy-llm-wiki-knowledge-base-guide)
