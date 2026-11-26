# Super Enfermagem: Caminho para o Maria Amélia - Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a complete, highly-polished, single-file HTML5/Canvas 2D platformer game in the style of "Super Mario Bros", starring heroic black nurse Maria Garcia and health mentor Ryan Garcia on their journey to Hospital Maria Amélia Buarque de Hollanda.

**Architecture:** Single self-contained HTML5 file (`index.html`) with embedded responsive CSS, HTML5 Canvas game loop, custom retro Web Audio API synthesizer, procedural sprite rendering engine (vector/pixel-art hybrid with zero external dependencies), tilemap/physics engine with sub-pixel collision resolution, and dual keyboard/touch controls.

**Tech Stack:** HTML5, Modern CSS3 (Flexbox/Grid, responsive canvas scaling, mobile virtual gamepad), Vanilla JavaScript (Canvas 2D Context, Web Audio API, RequestAnimationFrame game loop).

**Protagonist:** Nurse Maria Garcia (Black woman nurse, skin `#4A2511`/`#5C3018`, afro puff/surgical cap, emerald green scrub `#00875A`, white lab coat, stethoscope, white sneakers).

**Health Mentor NPC:** Ryan Garcia (Bald, bearded, moreno/tan skin `#8D5524`, navy scrub `#0369A1`, holding clinical clipboard, waving hand animation, interactive speech bubble sharing health and nursing tips).

---

## Tasks

- [x] **Task 1: Project Scaffolding & Responsive Single-File Architecture**
  - Create `index.html` structure with `<meta name="viewport">`, modern CSS resetting, pixel-art styling (`image-rendering: pixelated`), canvas container with fixed internal resolution (960x540) and responsive CSS scaling.
  - Implement mobile on-screen virtual touch controls (D-Pad Left/Right, Jump Button A, Action/Shoot Button B) with touch event listeners and preventDefault to avoid zoom/scrolling.
  - Setup core game state manager (`TITLE`, `PLAYING`, `PAUSED`, `STAGE_CLEAR`, `GAME_OVER`).

- [x] **Task 2: Web Audio API Retro Sound Effects Synthesizer**
  - Build an embedded procedural audio engine using `AudioContext` with zero external audio files.
  - Synthesize custom retro 8-bit sound effects:
    - Jump (pitch-bend square wave)
    - Big Jump (fuller harmonic bend)
    - Coin / Health Heart collect (crisp two-tone arpeggio: 987Hz -> 1318Hz)
    - Power-up spawn (staccato ascending chime)
    - Power-up acquire / Growth (triumphant brass-like synth arpeggio)
    - Power-down / Hurt (descending pitch sweep with noise)
    - Stomp enemy (short crunchy noise burst + low square blip)
    - Disinfectant Spray / Shoot (white noise burst + resonant sweep)
    - Flagpole slide & Victory fanfare (festive melody honoring Hospital Maria Amélia Buarque de Hollanda)
    - Game Over chime (gentle nostalgic descending melody)
    - Speech bubble chime blip
  - Audio mute/unmute toggle integrated into UI and HUD.

- [x] **Task 3: Procedural Character Drawing Engine (Maria Garcia)**
  - Procedural drawing functions for Nurse Maria Garcia in 3 visual states:
    - **Normal State:** Compact 24x36px proportions, rich brown skin tones (`#4A2511`/`#5C3018`), cute stylized afro puff, emerald scrub, white coat, stethoscope, sneakers.
    - **Super State:** 30x54px proportions, reinforced white lab coat, badge, subtle medical aura, stethoscope, heroic stance.
    - **Sanitizer State:** Distinctive cyan/white accents with glowing disinfectant pouch, sanitizing aura.
  - Dynamic animation states:
    - Idle (breathing/coat sway)
    - Run cycle (4-frame leg & arm swing with stethoscope bounce)
    - Jump (dynamic airborne pose with coat flair)
    - Skidding / turning pose
    - Growth / Shrink flashing transition (scale + color alternation)
    - Damage invulnerability flashing (alpha sine wave for 2 seconds)

- [x] **Task 3.1: NPC Ryan Garcia Drawing & Interactive Dialogue System**
  - Procedural rendering of Ryan Garcia: bald head with glossy highlight, dark full beard and mustache, warm moreno skin (`#8D5524`), navy blue clinical scrub, stethoscope, clipboard and waving hand.
  - Proximity detection system with Maria Garcia: when close, Ryan faces Maria, waves his hand, plays a soft audio chime, and renders a floating speech bubble.
  - Dynamic cycling speech bubble with educational quotes on nursing, hand hygiene, Florence Nightingale, and patient care.

- [x] **Task 4: Mario-Style Platform Physics Engine & Input Handling**
  - Continuous physics loop: horizontal velocity with acceleration, top speed, friction/deceleration, skidding when reversing direction.
  - Realistic platformer gravity with terminal velocity.
  - Variable jump height (holding jump key prolongs upward velocity, releasing cuts vertical velocity for snappy short hops).
  - Robust AABB collision resolution with the tilemap (resolves X and Y independently to eliminate corner-catching/snagging bugs).
  - Platform edge detection and one-way platform / solid ground handling.

- [x] **Task 5: World Generation, Tilemap & Interactive Blocks**
  - Level design featuring the urban parkway transitioning to the health district of Hospital Maria Amélia Buarque de Hollanda.
  - Tile types:
    - Ground & grass tiles (emerald green grass top, clean earth/sidewalk).
    - Medical brick blocks (bounces upwards on hit; shatters into 4 debris fragments when hit by Super Maria Garcia).
    - Golden `[?]` blocks (gleaming animated question/Nightingale lantern mark, bumps up on hit, ejects items, becomes solid stone).
    - Hospital Oxygen Cylinders / Green Pipes (various heights, solid obstacles that can be stood upon).
    - Abysses / urban pitfalls.
  - Multi-layer parallax background (sky gradient, distant sun, clouds, hospital district skyline, urban trees).

- [x] **Task 6: Power-ups, Collectibles & Sanitizing Spray Projectiles**
  - **Golden Nightingale Lamp / First-Aid Kit:** Pops out of `[?]` block, moves horizontally along platforms with gravity, bounces off walls. Collect to become Super Nurse.
  - **Álcool em Gel / Seringa Power-up:** Pops out of high-tier `[?]` blocks, grants Sanitizer state.
  - **Sanitizer Projectiles:** Press `X` or Fire button to shoot arcing green/cyan sanitizing droplets that bounce along the floor and eliminate germs.
  - **Health Drops / Green Hearts:** Floating collectible items throughout the level and inside blocks; score points and grant extra life every 50 collected.
  - Floating score indicators (`+100`, `+1000`, `1-UP!`) rising and fading.

- [x] **Task 7: Enemy AI & Combat Mechanics**
  - **Purple Bacteria (Goomba equivalent):** Wanders back and forth, reverses on wall/pipe contact. Stompable from above with player bounce.
  - **Super-Spiny Bacteria / Infecção (Spiny equivalent):** Sharp mutated virus spikes. Hurting if jumped on; must be eliminated with sanitizing spray or avoided.
  - Enemy squashing animation and falling off-screen when defeated.
  - Damage resolution: Super/Sanitizer state downgrades to Normal with 2s invincibility; Normal state loses life.

- [x] **Task 8: Hospital Maria Amélia Buarque de Hollanda Destination & Victory Sequence**
  - Grand hospital architectural facade at level's end:
    - Big illuminated marquee sign: **"HOSPITAL MARIA AMÉLIA BUARQUE DE HOLLANDA"**
    - Large green/white medical cross emblem
    - Double automatic hospital doors
    - Tall flagpole with emerald green nursing flag
  - Flagpole sequence: Maria Garcia grabs the pole, flag slides down, Maria slides down, dismounts, and walks into the hospital doors.
  - Victory celebration:
    - Procedural fireworks in emerald green, gold, and white bursting across the sky.
    - Congratulatory tribute honoring Nursing and the healthcare team at Hospital Maria Amélia Buarque de Hollanda.
    - Score tally screen with replay button.

- [x] **Task 9: HUD, UI Screens, Polish & Verification**
  - Retro pixel-styled HUD: `MARIA GARCIA` / `SCORE`, `💚 x 00`, `HOSPITAL: AMÉLIA`, `TEMPO 300`, `VIDAS x3`.
  - Title Screen with portrait of Maria Garcia, instructions, controls guide, and "Iniciar Plantão" button.
  - Game Over screen with restart option.
  - Comprehensive browser testing and syntax/runtime verification to ensure 60 FPS performance and bug-free gameplay.
