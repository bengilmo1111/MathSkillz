Original prompt: Build a personalised maths learning web game for "Henry Gilmore", a Year 4 New Zealand student who loves Pokemon, Ninjago Lego, and Minecraft. Follow the NZ Year 4 Mathematics and Statistics curriculum teaching sequence. Game should be visually appealing, addictive, and encourage repeated play.

## Architecture
- Single index.html with embedded CSS/JS (~1200 lines)
- 5 curriculum strands as game "worlds": Number (17 topics), Algebra (4), Measurement (9), Geometry (5), Statistics (4)
- Pokemon battle system for math problems (enemy creatures, HP bars, attacks)
- Ninjago ninja belt/rank progression (White through Black belt)
- Minecraft pixel-border visual style with pixel font
- LocalStorage for save data (key: mathskillz_henry_v2)
- Adaptive difficulty via attempt count
- XP, leveling, streaks, achievements, collectible creatures
- Particle system for visual flair (correct answer particles, confetti on victory)
- Web Audio API synth for sound effects

## Completed
- [x] Full game framework (screens: title, map, topics, battle, results, stats)
- [x] All 39 curriculum topics with question generators
- [x] Battle system with 10 questions per round, timer, HP bar
- [x] Multiple choice and typed answer support
- [x] Answer deduplication (no duplicate options)
- [x] Correct/wrong feedback with animations
- [x] Enemy hit effects, screen shake, particle explosions
- [x] XP & leveling system (XP per level = 80 + level * 20)
- [x] Belt progression (every 5 levels)
- [x] 15 collectible creatures earned through mastery
- [x] 13 achievements
- [x] Daily streak tracking
- [x] World unlock progression (50% completion required)
- [x] Star rating (1-3 stars based on score percentage)
- [x] Sound effects (correct, wrong, level up, boss, click)
- [x] Responsive design
- [x] Fullscreen toggle (F key)
- [x] Save/load via localStorage
- [x] Streak banner display
- [x] Victory confetti particles
- [x] Achievement toast notifications

## Known Issues / TODOs for next agent
- Consider adding more Pokemon-themed creature names and sprites
- Could add Minecraft crafting mechanic (combine mastered skills to unlock bonus content)
- Could add Ninjago elemental powers as special abilities in battle
- Timer speed could increase with difficulty
- Could add a "speed round" mode for practiced topics
- Mobile touch input is functional but could be enhanced
- Could add more question variety (e.g., visual fraction questions, clock face rendering)
- Boss battles (3rd+ attempt) could have unique mechanics
- Could add a leaderboard or parent dashboard

## 2026-02-12 Creative + Gameplay Iteration (playtested)

### Playtest findings vs modern indie web game quality
- Core loop is fun, but missing stronger short-cycle meta goals and tactical decisions.
- World/strand identity needed clearer on battle and topic screens.
- Replayability gap after first completion pass (needed a faster repeat mode).
- Achievement bug: combo achievement checked lifetime correct answers instead of combo performance.
- Automation stability issue: `window.advanceTime` was a no-op, reducing deterministic test reliability.

### Implemented this pass
- [x] Added world biome identity text and themed battle context (`Electric City`, etc.).
- [x] Added `Speed Round` mode button on completed topics (7 questions, faster timer, bonus XP).
- [x] Added battle mode indicator in header (biome + speed mode label).
- [x] Added Elemental Power system in battle:
  - Charge meter (`0/5`) gained from correct answers and combo milestones.
  - `Shield` (1 charge): blocks one miss/timeout combo break.
  - `Time Freeze` (2 charge): restores +8s timer.
  - `Storm Strike` (2 charge): direct enemy HP damage.
- [x] Added combo burst feedback callouts for charge milestones and power usage.
- [x] Added Daily Ninja Quest panel on map:
  - Goals: Answer 10, Correct 8, Win 1 battle.
  - Claim reward button grants +30 XP once complete.
- [x] Fixed combo achievement logic to use best combo (`bestCombo >= 5`) instead of `totalCorrect >= 50`.
- [x] Added `bestCombo` and `totalWins` tracking in save/state and stats.
- [x] Added inline SVG favicon to remove favicon 404 console noise.
- [x] Implemented functional `window.advanceTime(ms)` (promise-based delay) for test loop compatibility.

### Playtest verification completed
- [x] Manual full flow with Playwright MCP:
  - Title -> map -> world -> topic -> battle -> results -> topic -> map.
  - Verified world biome labels visible in topic and battle header.
  - Verified speed button appears after repeated completion.
  - Verified charge meter increments with correct answers.
  - Verified Shield blocks miss without normal penalty feedback.
  - Verified Time Freeze and Storm Strike consume charge and update UI/state.
  - Verified no new console errors.
- [x] Scripted Playwright client run with screenshot/state capture (`output/web-game/playtest-final`).

### Remaining ideas / next agent
- Add a dedicated visual timer value to make Time Freeze impact more obvious.
- Add one optional “Quest Complete” celebration overlay when daily reward is claimable.
- Add a small parent dashboard card for world-by-world readiness and weak-skill hints.
