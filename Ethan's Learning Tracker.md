# Ethan's Roblox Learning Tracker
**Learner:** Ethan, age 10 | **Parent:** Jonathan (jonathancyman@gmail.com)
**Project:** Pastel Rainbow Obby | **Started:** 2026-06-11

---

## Current Level: Beginner → Early Intermediate

Ethan started with no scripting experience and is now writing and debugging real Lua code independently, reading error messages, and understanding why things work — not just copying code.

---

## Skills Learned

### Roblox Studio — Environment
| Skill | Status | Notes |
|-------|--------|-------|
| Navigating the Explorer panel | ✅ Confident | Knows how to expand/collapse, find parts |
| Adding Scripts to Parts | ✅ Confident | Knows script must be inside the correct part |
| Properties panel (Anchored, Transparency) | ✅ Confident | Used to fix falling/invisible parts |
| Play/Stop testing in Studio | ✅ Confident | Learned not to test via Roblox app |
| Duplicating parts | ✅ Confident | Used to create platform gauntlet |
| Publishing a game | ✅ Confident | Game is now **Public** — done! |
| Publishing updates to a live game | ✅ Understands | File → Publish to Roblox (Alt/Option+P) pushes changes live |
| Audience reach / publishing fee | 🔄 Introduced | Roblox now requires a refundable fee + evaluation to reach All Ages (parent task) |

### Lua Scripting Concepts
| Concept | Status | Notes |
|---------|--------|-------|
| `script.Parent` | ✅ Confident | Understands it means "the part this script lives in" |
| `local` variables | ✅ Confident | Uses consistently |
| `while true do` loops | ✅ Confident | Core of all movement scripts |
| `task.wait()` | ✅ Confident | Controls speed of loops |
| `for` loops with step | ✅ Confident | Used in CFrame movement |
| `CFrame` for movement | ✅ Confident | Preferred method — carries players |
| `CFrame.Angles` for spinning | ✅ Confident | Used in spinning kill bricks |
| `Touched:Connect()` events | ✅ Confident | Used in kill bricks |
| `FindFirstChild("Humanoid")` | ✅ Understands | Used to detect players vs other objects |
| `humanoid.Health = 0` | ✅ Confident | Kill brick mechanic |
| Case sensitivity in naming | ✅ Hard learned | Debugged `Part1` vs `part1` himself |
| `game.Workspace.partName` | ✅ Confident | Knows how to reference parts by name |
| `Transparency` and `CanCollide` | ✅ Confident | Used in disappearing platforms — done! |
| `AssemblyLinearVelocity` (launch player) | 🔄 Developing | Core of jump pads — debugging placement, not yet live |
| `FindFirstChildWhichIsA("Humanoid")` | 🔄 Introduced | Safer player detection in jump pad script |
| Gamepasses / MarketplaceService | ⬜ Not started | Next major topic |

### Debugging Skills
| Skill | Status | Notes |
|-------|--------|-------|
| Reading error messages | 🔄 Developing | Beginning to notice Output panel |
| Using `print()` to debug | 🔄 Introduced | Learned to add a "BOING"/"ALIVE" print to check if code runs |
| Script vs LocalScript distinction | 🔄 Developing | Learned LocalScripts don't run inside parts; checked icon color |
| Checking Enabled / CanTouch / Anchored | 🔄 Introduced | Walked through these as bug suspects on the jump pad |
| Fixing case sensitivity bugs | ✅ Confident | Solved this independently |
| Identifying wrong script location | ✅ Confident | Learned script must be in the right part |
| Testing incrementally | 🔄 Developing | Tends to change many things at once |

---

## Scripts Mastered

### 1. Kill Brick
```lua
local killBrick = script.Parent
killBrick.Touched:Connect(function(hit)
    local humanoid = hit.Parent:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.Health = 0
    end
end)
```

### 2. Moving Platform (CFrame method — carries player)
```lua
local main = script.Parent
main.Anchored = true
local startPos = main.CFrame
local moveDistance = 50
while true do
    for i = 0, moveDistance, 0.1 do
        main.CFrame = startPos * CFrame.new(i, 0, 0)
        task.wait(0.01)
    end
    for i = moveDistance, 0, -0.1 do
        main.CFrame = startPos * CFrame.new(i, 0, 0)
        task.wait(0.01)
    end
end
```
> **Note:** TweenService and BodyPosition were tried first — both failed to carry the player. CFrame method is the correct approach.

### 3. Spinning Kill Brick
```lua
local brick = script.Parent
brick.Anchored = true
brick.Touched:Connect(function(hit)
    local humanoid = hit.Parent:FindFirstChild("Humanoid")
    if humanoid then humanoid.Health = 0 end
end)
while true do
    brick.CFrame = brick.CFrame * CFrame.Angles(0, math.rad(2), 0)
    task.wait(0.01)
end
```

### 4. Disappearing Platform
```lua
local platform = script.Parent
local waitTime = 1.5

platform.Touched:Connect(function(hit)
    if hit.Parent:FindFirstChild("Humanoid") then
        task.wait(waitTime)
        platform.Transparency = 1
        platform.CanCollide = false
        task.wait(2)
        platform.Transparency = 0
        platform.CanCollide = true
    end
end)
```
> **Note:** Part must be Anchored. Lower `waitTime` (e.g. 0.5) for a harder, faster-vanishing platform.

### 5. Bouncy Jump Pad (code correct — placement being debugged)
```lua
local pad = script.Parent

pad.Touched:Connect(function(hit)
    local character = hit.Parent
    local humanoid = character:FindFirstChildWhichIsA("Humanoid")
    if humanoid then
        local hrp = character:FindFirstChild("HumanoidRootPart")
        if hrp then
            hrp.AssemblyLinearVelocity = Vector3.new(0, 250, 0)
        end
    end
end)
```
> **Note:** This code is verified correct. The bug Ethan hit (no bounce, no print) was NOT the code — it was almost certainly the script sitting in the wrong Part (or CanTouch off). The reliable fix: click the pad Part first, THEN add the Script via the ➕ so it nests in the right part. Increase the `250` value to bounce higher.

---

## Session Log

### Session 2 — 2026-06-12
**Topics covered:** Disappearing platforms, making the game Public, Roblox audience reach & the new refundable publishing fee, publishing updates to a live game, bouncy jump pads (`AssemblyLinearVelocity`), `print()` debugging, Script vs LocalScript

**Breakthroughs:**
- Got the game set to **Public** — a real milestone
- Worked through a methodical debugging process on the jump pad (script type → Disabled flag → CanTouch → part location) instead of giving up

**Struggles:**
- Jump pad wouldn't fire even though the code was correct; root cause was almost certainly script placement in the wrong Part. Left as a fresh-start rebuild (click part → add script via ➕)
- Got frustrated and repeatedly asked for "new code" when the code wasn't the problem — a good teaching moment that bugs aren't always in the code itself

**Attitude:** Eager and fast-moving; wanted to jump between features. Persistence is strong but tends to want a code rewrite rather than isolating the environmental cause.

### Session 1 — 2026-06-11
**Topics covered:** Publishing a game, kill bricks, moving platforms (3 approaches), spinning kill bricks, moving platform gauntlet, how Roblox Studio testing works

**Breakthroughs:**
- Independently identified the case sensitivity bug (`Part1` vs `part1`)
- Persisted through 5+ failed approaches to get the moving platform working
- Understood why CFrame carries the player but TweenService doesn't

**Struggles:**
- Script location — took many attempts to get script inside the right part
- Distinguishing Studio editing mode from playing other games

**Attitude:** Extremely persistent, asks great questions, takes ownership of debugging

---

## Suggested Next Topics (in order)

1. **Finish the bouncy jump pad** — rebuild fresh (click part → add Script via ➕) and confirm the BOING. Reinforces script placement and `AssemblyLinearVelocity`.
2. **Speed Boost gamepass** — introduces `MarketplaceService`, `Players.PlayerAdded`, `CharacterAdded`. Requires parental involvement for Roblox account setup.
3. **Checkpoints / respawn system** — teaches `SpawnLocation` and stage management. High practical value for an obby.
4. **Badges** — free, easy, great for engagement. Teaches `BadgeService`.
5. **GUI elements** — stage counter, leaderboard. Introduces `ScreenGui`, `TextLabel`.

### ✅ Completed
- **Disappearing platforms** — done (`Transparency`, `CanCollide`)
- **Make game Public** — done (now awaiting Roblox evaluation to reach All Ages)

---

## Parent Notes
- Ethan is ready for slightly more complex scripts with 2-3 concepts combined
- He learns best by doing — show the script, let him type it, debug together
- He responds well to understanding *why* something works, not just copying
- Short sessions (30-45 min) seem optimal based on attention pattern
