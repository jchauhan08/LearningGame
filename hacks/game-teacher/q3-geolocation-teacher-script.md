---
layout: post
title: "Game Teacher Script (Rishabh) – Pseudocode + Robot Code"
description: "Teacher persona + instructional dialogue + stop-specific hints for Pseudocode and Robot Code challenges (AP CSP aligned)."
permalink: /hacks/game-teacher/q3-geolocation
comments: false
---

<style>
  /*
    Page-scoped "game HUD" styling
    - CSS-only, no JS
    - Respects prefers-reduced-motion
    - Kept local to this page via class prefixes
  */

  .lg-quest {
    --lg-ink: #e9f1ff;
    --lg-muted: rgba(233, 241, 255, 0.78);
    --lg-bg: rgba(8, 12, 24, 0.68);
    --lg-panel: rgba(10, 14, 28, 0.76);
    --lg-border: rgba(159, 241, 255, 0.24);
    --lg-accent: rgba(159, 241, 255, 0.95);
    --lg-accent2: rgba(200, 182, 255, 0.95);
    --lg-gold: rgba(255, 224, 138, 0.95);
    color: var(--lg-ink);
    font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
  }

  .lg-quest a {
    color: var(--lg-accent);
    text-decoration-thickness: 2px;
    text-underline-offset: 3px;
  }

  .lg-quest p,
  .lg-quest li {
    color: var(--lg-muted);
    line-height: 1.55;
  }

  .lg-quest code {
    background: rgba(0, 0, 0, 0.25);
    border: 1px solid rgba(255, 255, 255, 0.08);
    border-radius: 8px;
    padding: 0.12em 0.35em;
    color: rgba(255, 255, 255, 0.92);
  }

  .lg-hud {
    position: relative;
    margin: 10px 0 18px;
    padding: 14px 14px;
    border-radius: 18px;
    border: 1px solid var(--lg-border);
    background: linear-gradient(180deg, rgba(10, 14, 28, 0.80), rgba(8, 12, 24, 0.66));
    box-shadow:
      0 18px 45px rgba(0, 0, 0, 0.35),
      0 0 0 1px rgba(255, 255, 255, 0.04) inset;
    overflow: hidden;
  }

  /* scanlines + vignette */
  .lg-hud::after {
    content: "";
    position: absolute;
    inset: 0;
    background:
      linear-gradient(to bottom, rgba(255, 255, 255, 0.06), transparent 18%),
      repeating-linear-gradient(
        to bottom,
        rgba(255, 255, 255, 0.045) 0px,
        rgba(255, 255, 255, 0.045) 1px,
        transparent 3px,
        transparent 6px
      );
    opacity: 0.20;
    pointer-events: none;
    mix-blend-mode: overlay;
  }

  .lg-hud-title {
    display: flex;
    align-items: baseline;
    justify-content: space-between;
    gap: 10px;
    flex-wrap: wrap;
    margin: 0 0 10px;
  }

  .lg-hud-title h1 {
    margin: 0;
    font-size: 1.05rem;
    letter-spacing: 0.6px;
    text-transform: uppercase;
    color: rgba(233, 241, 255, 0.96);
    text-shadow: 0 0 18px rgba(159, 241, 255, 0.28);
  }

  .lg-chip {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 6px 10px;
    border-radius: 999px;
    border: 1px solid rgba(255, 255, 255, 0.10);
    background: rgba(0, 0, 0, 0.22);
    color: rgba(233, 241, 255, 0.86);
    font-weight: 700;
    letter-spacing: 0.2px;
  }

  .lg-chip b {
    color: var(--lg-gold);
    font-weight: 800;
  }

  .lg-teacher-banner {
    position: relative;
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    align-items: center;
    margin: 14px 0 18px;
    padding: 12px 12px;
    border-radius: 16px;
    border: 1px solid rgba(0, 0, 0, 0.08);
    background: linear-gradient(
      135deg,
      rgba(255, 224, 138, 0.20),
      rgba(159, 241, 255, 0.18),
      rgba(200, 182, 255, 0.16)
    );
    overflow: hidden;
  }

  /* Subtle animated glow + sparkles behind the badges */
  .lg-teacher-banner::before {
    content: "";
    position: absolute;
    inset: -40px;
    background:
      radial-gradient(circle at 20% 20%, rgba(159, 241, 255, 0.30), transparent 55%),
      radial-gradient(circle at 80% 30%, rgba(200, 182, 255, 0.26), transparent 55%),
      radial-gradient(circle at 35% 85%, rgba(255, 224, 138, 0.26), transparent 60%);
    filter: blur(10px);
    opacity: 0.85;
    pointer-events: none;
    transform: translateZ(0);
  }

  .lg-teacher-banner::after {
    content: "";
    position: absolute;
    inset: 0;
    background:
      radial-gradient(rgba(255,255,255,0.75) 1px, transparent 1.6px),
      radial-gradient(rgba(255,255,255,0.55) 1px, transparent 1.8px);
    background-size: 26px 26px, 38px 38px;
    background-position: 0 0, 12px 18px;
    opacity: 0.22;
    pointer-events: none;
    transform: translateZ(0);
  }

  @media (prefers-reduced-motion: no-preference) {
    .lg-teacher-banner {
      animation: lg-fade-up 450ms ease-out both;
    }

    .lg-teacher-banner::before {
      animation: lg-glow-drift 7.5s ease-in-out infinite;
    }

    .lg-teacher-banner::after {
      animation: lg-sparkle-pan 9s linear infinite;
    }
  }

  .lg-teacher-badge {
    position: relative;
    display: inline-block;
    padding: 8px 12px;
    border-radius: 999px;
    font-weight: 700;
    letter-spacing: 0.2px;
    color: #0b1020;
    background: linear-gradient(110deg, #ffe08a, #9ff1ff, #c8b6ff, #ffe08a);
    background-size: 300% 100%;
    box-shadow: 0 8px 22px rgba(0, 0, 0, 0.12);
    isolation: isolate;
    will-change: transform;
  }

  /* Shimmer highlight (subtle) */
  .lg-teacher-badge::before {
    content: "";
    position: absolute;
    inset: 0;
    border-radius: inherit;
    background: linear-gradient(
      120deg,
      transparent 0%,
      rgba(255, 255, 255, 0.55) 35%,
      transparent 70%
    );
    transform: translateX(-120%);
    opacity: 0.25;
    pointer-events: none;
    z-index: -1;
  }

  @media (prefers-reduced-motion: no-preference) {
    .lg-teacher-badge {
      animation:
        lg-gradient-shift 6s ease-in-out infinite,
        lg-badge-float 3.8s ease-in-out infinite;
    }

    .lg-teacher-badge:nth-child(2) {
      animation-delay: 120ms, 220ms;
    }

    .lg-teacher-badge::before {
      animation: lg-shimmer 3.6s ease-in-out infinite;
    }
  }

  /* Micro-interaction: feels more "game" */
  .lg-teacher-badge:hover {
    transform: translateY(-2px) scale(1.03);
  }

  .lg-teacher-badge:active {
    transform: translateY(0) scale(0.99);
  }

  /* Animated underline for headings on this page */
  .lg-quest h2,
  .lg-quest h3 {
    position: relative;
    padding: 10px 12px;
    border-radius: 14px;
    border: 1px solid rgba(159, 241, 255, 0.18);
    background: rgba(0, 0, 0, 0.18);
    box-shadow: 0 10px 24px rgba(0, 0, 0, 0.18);
    text-transform: uppercase;
    letter-spacing: 0.6px;
    color: rgba(233, 241, 255, 0.94);
  }

  .lg-quest h2::after,
  .lg-quest h3::after {
    content: "";
    display: block;
    height: 3px;
    width: 84px;
    margin-top: 6px;
    border-radius: 999px;
    background: linear-gradient(90deg, rgba(159,241,255,0.9), rgba(200,182,255,0.9), rgba(255,224,138,0.9));
    background-size: 200% 100%;
    opacity: 0.85;
  }

  @media (prefers-reduced-motion: no-preference) {
    .lg-quest h2::after,
    .lg-quest h3::after {
      animation: lg-underline-shift 3.2s ease-in-out infinite;
    }
  }

  /* Quest log bullets */
  .lg-quest ul {
    list-style: none;
    padding-left: 0;
  }

  .lg-quest ul > li {
    position: relative;
    padding-left: 26px;

  /* Interactive components (JS-free) */
  .lg-tabs {
    margin: 14px 0 18px;
  }

  .lg-tabs input[type="radio"] {
    position: absolute;
    opacity: 0;
    pointer-events: none;
  }

  .lg-tab-labels {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    margin-bottom: 10px;
  }

  .lg-tabs label {
    cursor: pointer;
    user-select: none;
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 10px 12px;
    border-radius: 14px;
    border: 1px solid rgba(255, 255, 255, 0.12);
    background: rgba(0, 0, 0, 0.22);
    color: rgba(233, 241, 255, 0.86);
    font-weight: 800;
    letter-spacing: 0.4px;
    text-transform: uppercase;
    box-shadow: 0 10px 22px rgba(0, 0, 0, 0.18);
  }

  #lg-tab-pseudo:checked ~ .lg-tab-labels label[for="lg-tab-pseudo"],
  #lg-tab-robot:checked ~ .lg-tab-labels label[for="lg-tab-robot"],
  #lg-tab-teacher:checked ~ .lg-tab-labels label[for="lg-tab-teacher"] {
    border-color: rgba(159, 241, 255, 0.38);
    background: linear-gradient(180deg, rgba(159, 241, 255, 0.12), rgba(0, 0, 0, 0.22));
    color: rgba(233, 241, 255, 0.95);
    text-shadow: 0 0 18px rgba(159, 241, 255, 0.22);
  }

  .lg-tab-panels {
    border-radius: 18px;
    border: 1px solid rgba(159, 241, 255, 0.18);
    background: rgba(0, 0, 0, 0.16);
    padding: 12px 12px;
  }

  .lg-tab-panel {
    display: none;
  }

  #lg-tab-pseudo:checked ~ .lg-tab-panels .lg-tab-panel-pseudo {
    display: block;
  }

  #lg-tab-robot:checked ~ .lg-tab-panels .lg-tab-panel-robot {
    display: block;
  }

  #lg-tab-teacher:checked ~ .lg-tab-panels .lg-tab-panel-teacher {
    display: block;
  }

  .lg-card {
    border-radius: 16px;
    border: 1px solid rgba(255, 255, 255, 0.10);
    background: rgba(8, 12, 24, 0.62);
    padding: 12px 12px;
    margin: 10px 0;
  }

  details.lg-accordion {
    border-radius: 16px;
    border: 1px solid rgba(255, 255, 255, 0.12);
    background: rgba(0, 0, 0, 0.18);
    padding: 10px 12px;
    margin: 10px 0;
  }

  details.lg-accordion > summary {
    cursor: pointer;
    list-style: none;
    font-weight: 900;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    color: rgba(233, 241, 255, 0.92);
  }

  details.lg-accordion > summary::-webkit-details-marker {
    display: none;
  }

  details.lg-accordion > summary::after {
    content: " +";
    color: rgba(255, 224, 138, 0.95);
  }

  details.lg-accordion[open] > summary::after {
    content: " –";
    color: rgba(159, 241, 255, 0.95);
  }

  .lg-check {
    display: flex;
    align-items: flex-start;
    gap: 10px;
    padding: 10px 12px;
    border-radius: 14px;
    border: 1px solid rgba(255, 255, 255, 0.10);
    background: rgba(0, 0, 0, 0.18);
    margin: 8px 0;
  }

  .lg-check input {
    margin-top: 2px;
    accent-color: rgba(159, 241, 255, 0.95);
  }

  .lg-note {
    color: rgba(233, 241, 255, 0.72);
    font-size: 0.92rem;
    margin-top: 8px;
  }

  @media (prefers-reduced-motion: no-preference) {
    details.lg-accordion {
      transition: border-color 200ms ease;
    }
    details.lg-accordion[open] {
      border-color: rgba(159, 241, 255, 0.30);
    }
  }
    margin: 8px 0;
  }

  .lg-quest ul > li::before {
    content: "▸";
    position: absolute;
    left: 6px;
    top: 0;
    color: var(--lg-gold);
    text-shadow: 0 0 14px rgba(255, 224, 138, 0.25);
  }

  /* Make numbered dialogue feel like a quest step list */
  .lg-quest ol {
    padding-left: 22px;
  }

  .lg-quest ol > li {
    margin: 8px 0;
  }

  @keyframes lg-fade-up {
    from { opacity: 0; transform: translateY(8px); }
    to { opacity: 1; transform: translateY(0); }
  }

  @keyframes lg-glow-drift {
    0% { transform: translate3d(-8px, -4px, 0) scale(1.00); }
    50% { transform: translate3d(10px, 6px, 0) scale(1.03); }
    100% { transform: translate3d(-8px, -4px, 0) scale(1.00); }
  }

  @keyframes lg-sparkle-pan {
    0% { background-position: 0 0, 12px 18px; }
    100% { background-position: 120px 80px, 150px 110px; }
  }

  @keyframes lg-gradient-shift {
    0% { background-position: 0% 50%; }
    50% { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
  }

  @keyframes lg-badge-float {
    0% { transform: translateY(0); }
    50% { transform: translateY(-2px); }
    100% { transform: translateY(0); }
  }

  @keyframes lg-shimmer {
    0% { transform: translateX(-120%); }
    55% { transform: translateX(120%); }
    100% { transform: translateX(120%); }
  }

  @keyframes lg-underline-shift {
    0% { background-position: 0% 50%; }
    50% { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
  }
</style>

<div class="lg-teacher-banner" role="note" aria-label="Animated topic badges">
  <span class="lg-teacher-badge">Pseudocode</span>
  <span class="lg-teacher-badge">Robot Code</span>
</div>

<div class="lg-quest" markdown="1">

<div class="lg-hud" role="note" aria-label="Quest HUD">
  <div class="lg-hud-title">
    <h1>Quest Log: Code Runner Mastery</h1>
    <span class="lg-chip">Difficulty: <b>AP CSP</b></span>
  </div>
</div>

<div class="lg-tabs" aria-label="Interactive NPC console">
  <input type="radio" name="lg-tab" id="lg-tab-pseudo" checked>
  <input type="radio" name="lg-tab" id="lg-tab-robot">
  <input type="radio" name="lg-tab" id="lg-tab-teacher">

  <div class="lg-tab-labels" role="tablist" aria-label="Choose a console">
    <label for="lg-tab-pseudo" role="tab" aria-controls="lg-panel-pseudo">NPC: Pseudocode</label>
    <label for="lg-tab-robot" role="tab" aria-controls="lg-panel-robot">NPC: Robot Code</label>
    <label for="lg-tab-teacher" role="tab" aria-controls="lg-panel-teacher">Teacher Console</label>
  </div>

  <div class="lg-tab-panels">
    <div class="lg-tab-panel lg-tab-panel-pseudo" id="lg-panel-pseudo" role="tabpanel">
      <div class="lg-card">
        <b>Interact:</b> Click hints to reveal them.
        <div class="lg-note">These are for practice; checks aren’t saved.</div>
      </div>

      <details class="lg-accordion">
        <summary>Hint 1 — Nudge</summary>
        Underline the input and output. What must be true at the end?
      </details>

      <details class="lg-accordion">
        <summary>Hint 2 — More Direct</summary>
        Track your variables step-by-step. After each step, ask: what changed?
      </details>

      <details class="lg-accordion">
        <summary>Hint 3 — Outline</summary>
        Write the algorithm in three parts: setup → decision/loop → final output.
      </details>
    </div>

    <div class="lg-tab-panel lg-tab-panel-robot" id="lg-panel-robot" role="tabpanel">
      <div class="lg-card">
        <b>Interact:</b> Reveal the strategy one layer at a time.
        <div class="lg-note">Try to solve it before opening Hint 3.</div>
      </div>

      <details class="lg-accordion">
        <summary>Hint 1 — Nudge</summary>
        Describe the route out loud. Where should the robot turn?
      </details>

      <details class="lg-accordion">
        <summary>Hint 2 — More Direct</summary>
        Before moving, check if you can move. Use CAN_MOVE(direction) to avoid crashes.
      </details>

      <details class="lg-accordion">
        <summary>Hint 3 — Outline</summary>
        Use a repeat-until structure: keep moving forward when possible; otherwise rotate and try again.
      </details>
    </div>

    <div class="lg-tab-panel lg-tab-panel-teacher" id="lg-panel-teacher" role="tabpanel">
      <div class="lg-card">
        <b>Quest Objectives (check as you go)</b>
        <div class="lg-note">This is a local checklist for the player.</div>
      </div>

      <label class="lg-check"><input type="checkbox"> I identified the goal (what must be true at the end).</label>
      <label class="lg-check"><input type="checkbox"> I used variables correctly (set/update/use).</label>
      <label class="lg-check"><input type="checkbox"> I used conditionals for decisions.</label>
      <label class="lg-check"><input type="checkbox"> I used loops only when repetition is needed.</label>
      <label class="lg-check"><input type="checkbox"> I tested, adjusted, and re-ran my solution.</label>
    </div>
  </div>
</div>

## Pseudocode – Rebecca Yan, Meryl Chen
- Pseudocode is a plain-language, step-by-step way of describing how an algorithm or program works.
- Instead of using strict programming syntax, pseudocode focuses on logic and structure.
- This makes it easier for students to understand core computer science concepts such as assignment operators, conditional statements, loops, and logical reasoning.
- Our project uses a code runner to help teach students how pseudocode functions in a practical and interactive way.
- Rather than simply reading examples, students actively engage with pseudocode by writing their own solutions.
- This hands-on experience allows them to see how different logical choices affect program behavior, reinforcing computational thinking skills emphasized by the College Board.
- **Example artifact:** IMG_1273.pdf (provided separately)
- The questions presented to students are based entirely on topics outlined by the College Board for AP CSP.
- These include conditionals, variables, algorithm development, and logical flow.
- Many of the problems are adapted from College Board practice exam questions to ensure that the difficulty level and style closely match what students will encounter on the actual AP exam.
- The learning experience is embedded into a game environment.
- As players navigate through a maze, they encounter non-playable characters (NPCs).
- When a player interacts with an NPC, a new section appears that includes a code runner using pseudocode.
- To continue progressing through the maze, the player must correctly type out a College Board–level pseudocode solution that makes the algorithm work as intended.
- Correctly answering these questions allows players to move forward in the game, creating a clear connection between problem-solving and progression.
- After meeting a certain number of NPCs and successfully completing their pseudocode challenges, players are rewarded with a badge.
- This badge system serves as motivation and provides a sense of achievement while reinforcing learning through repeated practice and application.

## Q1: ROBOT CODE – Avantika & Shay
### What is robot code?
Robot code is a pseudocode language that consists of 4 commands:
- `MOVE_FORWARD()`
- `ROTATE_LEFT()`
- `ROTATE_RIGHT()`
- `CAN_MOVE(direction)`

Robot code is used as a basic intro to coding, breaking more complex functions into simple logical blocks. These small logical blocks allow for easier human understanding, helping the user visualize the actions.

### Robot code game layout
- One side of the screen has a code editor
- The other side of the screen has the robot and maze
- The user inputs code to control the robot to solve the maze

## User Guide Role: Rishabh (Content Specialist)
Responsible for drafting all instructional dialogue, creating stop-specific hints for both Robot Code and Pseudo Code challenges, and managing the “Teacher Data” dictionary.

## Teacher Tone / Personality
- **Supportive coach**: calm, clear, never sarcastic.
- **Scaffolded help**: starts with a nudge → then a concrete hint → then a near-solution outline.
- **No answer-dumping**: the teacher gives structure and checkpoints, not copy/paste solutions.
- **Player-respectful**: assumes the player is capable; celebrates effort and iteration.
- **Short sentences**: optimized for on-screen dialogue boxes.

## Teacher Script (Intro → Instructions → Hints → Feedback)

### 1) Intro (when a player starts a Pseudocode or Robot Code stop)
**Teacher (Game Teacher):**
1. “Welcome. This stop is about thinking like a computer—one clear step at a time.”
2. “You don’t need perfect syntax. You need clear logic and correct flow.”
3. “When you’re ready, run it and see what your choices do.”

### 2) Instructions (what the player should do)
**Teacher:**
1. “Read the prompt carefully. Identify what the algorithm must accomplish.”
2. “Write the steps in order. Use variables when you need to store or update information.”
3. “Use conditionals for decisions. Use loops for repeated steps.”
4. “Test and adjust. Small changes can completely change the outcome.”

### 3) Stop-Specific Hints
Use these when the player requests help.

#### A) Pseudocode stop hints
**Hint 1 (gentle nudge):**
- “Underline the input and output. What must be true at the end?”

**Hint 2 (more direct):**
- “Track your variables. After each step, ask: what changed?”

**Hint 3 (near-solution outline):**
- “Write the algorithm in three parts: setup → decision/loop → final output.”

#### B) Robot code stop hints
**Hint 1 (gentle nudge):**
- “Try describing the route out loud. Where should the robot turn?”

**Hint 2 (more direct):**
- “Before moving, check if you can move. Use `CAN_MOVE(direction)` to avoid crashes.”

**Hint 3 (near-solution outline):**
- “Use a repeat-until structure: keep moving forward when possible; otherwise rotate and try again.”

### 4) Feedback Dialogue

**On Correct / Complete:**
1. “Nice. That’s real-world defensive programming.”
2. “You didn’t just make it run—you made the logic clear and testable.”

**On Partially Correct (missing one case):**
1. “You’re close. I see error handling, but one scenario still falls through.”
2. “Check for: a missing branch, an off-by-one step, or a variable that never updates.”

**On Incorrect / Needs Retry:**
1. “Not quite yet—and that’s fine.”
2. “Goal check: do you have clear steps, and do your conditionals match the prompt?”

### 5) Post-Stop Explanation (after the player completes a challenge)
**Teacher:**
1. “Here’s the big idea: computers don’t guess. They follow steps.”
2. “Pseudocode helps you plan the logic without getting stuck on syntax.”
3. “Robot code trains you to break a big goal into repeatable moves and checks.”
4. “In game terms: clear thinking is how you earn progress and badges.”

### 6) Quick Checklist (teacher ‘grading’ rubric)
**Teacher:**
- “✅ Steps are in a logical order”
- “✅ Uses variables appropriately (set/update/use)”
- “✅ Conditionals match the prompt’s cases”
- “✅ Loops are used only when repetition is needed”
- “✅ Output/result is clearly produced”

## Optional Implementation Skeleton (legacy reference)
This file previously included a Geolocation error-handling reference. Per the “do not change any code” requirement, the code block below is preserved as-is.

Note: `showMessage(...)` and `showPosition(...)` are intentionally **app-specific helpers** (not browser built-ins).
- `showMessage(text)` should update your UI (a status banner, dialogue box, toast, etc.)
- `showPosition(position)` should render latitude/longitude (or whatever your game uses)

```js
// Example helper: wire this to your UI.
// Minimal version: writes to a <div id="geoMessage"></div> if it exists.
function showMessage(text) {
  const el = document.getElementById('geoMessage');
  if (el) el.textContent = text;
  else console.log(text);
}

function getLocation() {
  if (!navigator.geolocation) {
    showMessage('Geolocation is not supported in this browser.');
    return;
  }

  const options = { enableHighAccuracy: true, timeout: 5000, maximumAge: 0 };

  navigator.geolocation.getCurrentPosition(
    (position) => {
      showPosition(position);
    },
    (error) => {
      switch (error.code) {
        case 1:
          showMessage('Location permission denied. Please enable it in your browser settings.');
          break;
        case 2:
          showMessage('Location unavailable. Try again or check your connection/GPS.');
          break;
        case 3:
          showMessage('Location request timed out. Please try again.');
          break;
        default:
          showMessage('An unknown location error occurred.');
      }
    },
    options
  );
}
```

</div>
