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
