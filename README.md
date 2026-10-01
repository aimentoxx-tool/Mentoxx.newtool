# Xiaomi Bootloader Unlock Quota Helper

This project automates token collection and timed bootloader unlock requests for Xiaomi devices (Global flow), based on Xiaomi Community web/API behavior.

> [!WARNING]
> Use this project strictly at your own risk and your own responsibility.
> Even if queue/add-authorize appears successful, authorization is not guaranteed and may be coincidence.
> Constant, never-ending connected sessions might be noticed on Xiaomi's side, so avoid unnecessary nonstop activity and keep sessions practical.
> This project now includes a best-effort pre-refresh logout phase that attempts separate logout requests for the previous Chrome and Firefox sessions.
> Logout success is not guaranteed in all cases and depends on current server-side session state.
> Current mitigation is experimental: this logout flow is an attempt to reduce long-lived sessions, and each refresh cycle also uses a randomized timeout (`REFRESH_INTERVAL` +/-20%) to avoid fixed periodic behavior.

## What This Program Does

The toolchain is built around two main steps:

1. Collect valid login tokens from Xiaomi Community (`new_bbs_serviceToken` and `popRunToken`).
2. Send a precisely timed unlock request to Xiaomi's API around Beijing midnight (`UTC+8`) to improve timing against daily quota limits.

## Requirements

- Python 3.x
- Browser access to Xiaomi Community account
- Chrome WebDriver available for Selenium-based token extraction
- Internet access to NTP servers and Xiaomi API endpoints

The scripts can auto-install Python dependencies when missing.

## Basic Usage (Windows)

**Recommended — one-click launch:**

1. Double-click `AutoStart.bat`.
   - Detects `py` or `python` automatically and starts `AutoStart.py`.
2. `AutoStart.py` opens a new console for `AutoJobs.py`, places it in the left column, and closes itself.
3. Complete login prompts in Chrome and Firefox.
4. `AutoJobs.py` extracts tokens, updates `token.txt`, and starts/restarts 4 `NScript.py` windows automatically.

**Alternative — run directly from a terminal:**

```bash
py AutoStart.py
```


## Main Workflow

1. Run `AutoStart.bat` or `python AutoStart.py`.
2. `AutoStart.py` opens a new console for `AutoJobs.py`, places it in the left column, then closes the launcher window.
3. `AutoJobs.py` logs in to `https://c.mi.com/global` in Chrome and Firefox and extracts tokens.
4. `AutoJobs.py` writes tokens to `token.txt` (4 lines, reused by parallel runs).
5. Every refresh cycle, `AutoJobs.py` first closes running script windows, then sends best-effort logout requests for the previous Chrome and Firefox browser sessions.
6. `AutoJobs.py` obtains fresh tokens and starts 4 `NScript.py` windows arranged on the right side in a 2x2 grid.
7. `NScript.py`:
   - checks account unlock status via Xiaomi API,
   - synchronizes time with NTP servers,
   - applies an offset from `timeshift.txt`,
   - waits for target request moment,
   - sends POST requests to unlock endpoint and prints API response status.

## Window Layout (Windows)

The screen is divided into 3 equal columns.

**Phase 1 — Token collection (browser login):**

```
┌─────────────────┬─────────────────────────────────┐
│                 │                                 │
│   AutoJobs.py   │     Chrome / Firefox browser    │
│   (log/status)  │        (c.mi.com/global)        │
│                 │                                 │
└─────────────────┴─────────────────────────────────┘
   col 1 (1/3)              cols 2+3 (2/3)
```

**Phase 2 — Timed unlock requests (Script windows):**

```
┌─────────────────┬────────────────┬────────────────┐
│                 │   NScript.py   │   NScript.py   │
│   AutoJobs.py   │   (token #1)   │   (token #2)   │
│   (log/status)  ├────────────────┼────────────────┤
│                 │   NScript.py   │   NScript.py   │
│                 │   (token #3)   │   (token #4)   │
└─────────────────┴────────────────┴────────────────┘
   col 1 (1/3)         col 2 (1/3)      col 3 (1/3)
```

## File Roles
# Xiaomi Bootloader Unlock Quota Helper (Termux & Desktop)

This project automates token collection and timed bootloader unlock requests for Xiaomi devices (Global flow), based on Xiaomi Community web/API behavior.

> [!WARNING]
> Use this project strictly at your own risk and your own responsibility.
> Even if queue/add-authorize appears successful, authorization is not guaranteed and may be coincidence.
> Constant, never-ending connected sessions might be noticed on Xiaomi's side, so avoid unnecessary nonstop activity and keep sessions practical.
> Current mitigation is experimental: this logout flow is an attempt to reduce long-lived sessions, and each refresh cycle uses a randomized timeout (`REFRESH_INTERVAL` +/-20%) to avoid fixed periodic behavior.

---

## What This Program Does

The toolchain is built around two main steps:

1. **Token Collection**: Collect valid login tokens from Xiaomi Community (`new_bbs_serviceToken` and `popRunToken`).
2. **Quota Timing**: Send a precisely timed unlock request to Xiaomi's API around Beijing midnight (`UTC+8`) to maximize chances against daily quota limits.

---

## Installation & Usage

### 📱 Android (Termux Setup)

To set up and run the environment in Termux:

```bash
# Update packages and install prerequisites
pkg update && pkg upgrade -y
pkg install python git clang make -y

# Clone repository
git clone [https://github.com/matedon/Mentoxx.newtool.git](https://github.com/matedon/Mentoxx.newtool.git)
cd Mentoxx.newtool

# Install Python requirements
pip install -r requirements.txt

# Run token extractor
python GetTokens.py

# Or run the main tool directly
python Mentoxxnew.py
