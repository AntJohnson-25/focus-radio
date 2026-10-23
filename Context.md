# Project Context

## Outcome

Free "Focus Radio" for custombiztools.online: the user's own original tracks as a shuffled radio station, plus a Pomodoro timer and synthesized background sounds. Single static `index.html` + `tracks/*.mp3`, client-side only. Source stays closed, no LICENSE file.

## Current status

2026-10-22: v1 built. Screenshotted in headless Edge at 1280px and 500px (layout OK). Audio playback, ambient sounds, chime and break ducking NOT yet heard by a person. Not in git, not hosted.

2026-10-22 (later): UI redesigned to the "lo-fi night room" direction the user picked from 4 options. Full-screen night window behind everything (stars, moon, seeded SVG skyline with lit windows, rain canvas). Big glowing timer in the centre, player dock + Rain/Fireplace/Hum chips at the bottom, Up next + How it works below the fold. The scene reacts: the Rain slider makes the canvas rain heavier (light drizzle always on), Fireplace = flickering orange glow, Deep hum = breathing purple glow, break mode = mint accent + teal sky tint. Features/logic/IDs unchanged. v1 kept as `index.v1-backup.html`. Screenshotted at 1366x768 and 390px (via iframe).

2026-10-22: user approved the UI and said commit + push. Git repo `AntJohnson-25/focus-radio` (public, like the other tool repos; Pages on the free plan needs public). Pages = GitHub Actions (`.github/workflows/pages.yml`), enabled via `gh api`. Live at https://antjohnson-25.github.io/focus-radio/. `CNAME` file says `radio.custombiztools.online`, but the custom domain is NOT set in Pages yet because DNS doesn't exist (setting it first would redirect to a dead domain). Push with `git -c credential.helper= -c "credential.helper=!G:/GitHubCLI/bin/gh.exe auth git-credential" push` (GCM hangs). `index.v1-backup.html` is git-ignored.

2026-10-22 (evening): FULLY LIVE at https://radio.custombiztools.online. The user added the Namecheap CNAME, the custom domain is set, HTTPS is enforced (cert approved), and a live "F" card + JSON-LD entry is on custombiztools.online (landing repo d912bb2, replaced the "Coming soon" placeholder).

## Next action

Listen-test on iPhone at the live URL. More tracks when the user has them.

Also: user opens `index.html` (double-click works; no server needed) and listens: play/skip tracks, ambient sliders, start a 1-minute focus session (Timer settings) to hear the chime and the music duck on the break. Then launch per the custombiztools tool-launch process (suggested subdomain `radio.custombiztools.online`).

## Waiting on me

- [ ] Listen-test, including on iPhone.
- [ ] More tracks. Only 3 (~9 min) so far; the station repeats quickly. Add mp3s to `tracks/` and a line to `TRACKS` in `index.html` (file, title, dur in seconds). More stations later = more `TRACKS` lists.
- [ ] Create the GitHub repo + DNS record when ready to launch.

## Decisions

- Music plays through a plain `<audio>` element (not Web Audio) so lock-screen / media keys / background play work. Media Session metadata + handlers set.
- iPhone Safari ignores `audio.volume`: detected at load; volume slider is replaced by "use your device's volume buttons", and "Get quieter" on breaks falls back to "Pause".
- Ambient layers (Rain, Fireplace, Deep hum) are generated noise in Web Audio, no audio files. On iPhone they likely stop when the screen locks.
- Fades use setInterval, not rAF, so they finish in background tabs. Timer counts from a wall-clock end time, so throttled tabs stay accurate.
- Skip does not count a focus session as done. The session count resets on page reload.
- Settings saved in localStorage key `focusRadio.v1` (wrapped in try/catch).
- Chime = two soft sine notes, toggleable (user dislikes cheesy SFX).
- Shuffle never plays the same track twice in a row; "Up next" shows at most tracks-1 items.

## Open questions and risks

- 2026-10-22: user renamed it **"Custombiztool Focus Radio"** (their spelling, singular). Folder and localStorage key `focusRadio.v1` keep the old name on purpose. No favicon or share image yet.
- Each mp3 is about 4.3MB at 192kbps; fine for Pages, but a large library should be re-encoded to ~128kbps.
