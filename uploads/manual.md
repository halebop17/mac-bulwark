# MacBulwark User Manual

## Overview
MacBulwark is a macOS firewall app that lets you monitor and block outgoing network connections for individual apps. It uses a system-level network extension to ensure blocking persists even after quitting the main app.

## Main Features
- **App Blocking:** Add any application to the monitored list and block or allow all its network connections.
- **Domain Filtering:** View and manage which domains each app can access. Block or allow specific domains per app.
- **Persistent Blocking:** Blocking rules remain active at the system level, even if you quit MacBulwark. The extension enforces rules until you remove them in the app.
- **Trial Mode:** Without registration, you get a full-featured 24-hour trial with unlimited apps.
- **Registration:** Unlock unlimited blocking and remove the trial time limit by registering with a Payhip license key.

## First Launch — Enabling the Firewall

On first launch, the firewall extension is not yet active. At the bottom of the window you will see a status bar with **Firewall inactive** and an **Enable** button.

1. Click **Enable**.
2. macOS will ask for permission to install a system extension — click **Open System Settings**.
3. In System Settings → Privacy & Security, approve the MacBulwark extension.
4. The status bar will change to **Firewall active** (green dot).

> The extension only needs to be installed once. After approval it starts automatically on every boot, even when the main app is closed.

## How to Use

### 1. Add an App
- Click the **+** button in the top-right of the toolbar.
- A file picker opens to `/Applications`. Select any `.app` bundle and click **Add**.
- The app appears in the list with all connections in the **Blocked** state by default.

### 2. Block or Allow an App
- Each app row shows a pill-shaped status badge on the right:
  - **Blocked** (red) — all outgoing connections are dropped.
  - **Filtered** (yellow) — some connections allowed, some blocked.
  - **Allowed** (green) — all connections are allowed.
- Click the pill to change state:
  - Blocked → Allowed (immediate)
  - Allowed → Blocked (asks for confirmation)
  - Filtered → Blocked (asks for confirmation)

### 3. Manage Individual Domains
- Click anywhere on an app row to expand it and see its domain list.
- Each domain row shows:
  - A **green or red dot** — green means the connection is currently allowed, red means blocked.
  - The **domain name** (subdomain dimmed, base domain in full).
  - A **purpose tag** — see [Domain Purpose Labels](#domain-purpose-labels) below.
  - A **toggle button** (✓ / ✗) to allow or block that specific domain.
  - On hover: a **pencil icon** to edit the entry and an **× icon** to remove it.
- Click **Add domain** at the bottom of the list to manually add a domain rule.

### 4. Filter the App List
Use the segmented control in the toolbar to show **All**, **Blocked**, **Filtered**, or **Allowed** apps.

### 5. Network Monitor
Click the **Network Monitor** tab at the top of the window to see recent network activity.
- Each app row shows how many connections were made and how many were blocked in the last 5 minutes.
- The most recently contacted hostnames are listed beneath each app.
- Click **Block** next to any app to add it to the firewall list with all connections blocked.

### 6. Status Bar
The bottom bar shows:
- A coloured dot and label indicating the firewall extension state.
- A summary of how many apps are monitored and how many are blocked or filtered.

## Domain Purpose Labels

When a domain is added (manually or detected automatically), MacBulwark assigns it a purpose label so you can understand what each connection does at a glance.

| Label | What it means |
|---|---|
| **telemetry** | Crash and diagnostics data sent back to the developer |
| **analytics** | Usage statistics and behavioural tracking |
| **cdn** | Content delivery — images, fonts, static assets |
| **auth** | Login, OAuth, or identity verification |
| **sync** | Data sync with a cloud service or server |
| **account** | Account management and user profiles |
| **playback** | Audio or video streaming |
| **metadata** | Catalogue, search, or library data |
| **update** | Software update checks and downloads |
| **ai** | AI/ML inference endpoints |

Labels highlighted in **yellow** (telemetry and analytics) are considered sensitive — they typically send data about your behaviour to third parties.

## Trial Mode
When unregistered, MacBulwark operates in trial mode:
- You get **24 hours** of full app functionality.
- During those 24 hours, there is **no app-count limit**.
- After the timer expires, the app shows a lock screen and requires registration to continue.

A popup appears on first launch explaining trial mode and showing the countdown. You can dismiss it and continue using the app, or click **Register…** to unlock the full version immediately.

## Block Behavior and Logic (Detailed)

This section describes the exact enforcement model used by MacBulwark.

### Per-App State Model
- Each app has one state derived from its domain entries:
  - **Blocked**: zero allowed entries.
  - **Allowed**: all entries are allowed (or there are no entries).
  - **Filtered**: a mix of allowed and blocked entries.

### What the Extension Enforces
- MacBulwark exports rules to the system extension as follows:
  - **Allowed app**: not exported as a blocking rule.
  - **Blocked app**: exported with an empty blocked-domain list, which means block all traffic for that app.
  - **Filtered app**: exported with only explicitly blocked hostnames.

### Important Filtered Semantics
- **Filtered mode is denylist-based, not allowlist-based.**
- In Filtered mode, only domains marked as blocked are dropped.
- Any domain not explicitly blocked is allowed.
- If you remove or flip all blocked entries to allowed, the app effectively becomes Allowed.

### Adding and Editing Domains
- **Add domain** creates a per-domain rule for that app.
- New domain entries default to **allowed** unless you set them to blocked.
- You can edit subdomain/domain/purpose/allow-block state later.
- Domain host parts are normalized (trimmed/lowercased) and duplicate entries are deduplicated.

### Pill Click Behavior
- The state pill is intentionally asymmetric for safety:
  - **Blocked → Allowed** is immediate.
  - **Allowed/Filtered → Blocked** shows a destructive confirmation dialog first.
- Confirming applies full-app block immediately.
- Cancel leaves the current state unchanged.

### Practical Examples
- If Safari has `example.com` blocked and `vimeo.com` allowed, Safari is **Filtered**:
  - Requests to `example.com` are blocked.
  - Requests to `vimeo.com` are allowed.
  - Requests to domains not listed are allowed unless they are explicitly blocked by another rule entry.
- If you click the pill and confirm **Block All**, Safari becomes **Blocked** and all its traffic is dropped.

## Registration

### Get a License Key
Purchase a license from the official Payhip page.

### Register in the App
1. Open the **MacBulwark** menu in the menu bar and select **Register…**
2. Enter your Payhip license key in the dialog and click **Register**.
3. MacBulwark verifies the key with Payhip's servers. On success, a confirmation dialog shows your license key.
4. The trial lockout is removed immediately — no restart required.

### Update or Change License
You can re-open the registration dialog at any time (MacBulwark menu → Register…) to update your license key.

### Unregister a License
1. Open the **MacBulwark** menu and select **Register…** while already registered.
2. In the license dialog, click **Unregister**.
3. MacBulwark deactivates this machine on Payhip and removes the local license.
4. Trial mode resumes immediately until a valid key is entered again.

## Error Handling
- If license verification fails, an error message explains the problem (invalid key, network error, etc.). You can correct the key and try again.
- If the firewall extension cannot be installed, an error appears in the status bar. Check that macOS has approved the extension in System Settings → Privacy & Security.

## Persistent Blocking
- The system extension enforces blocking rules independently of the main app.
- You do not need to keep MacBulwark open for blocking to remain active.
- To remove a block: open MacBulwark, find the app, and click its pill badge to set it to **Allowed**, or click the **×** to remove it from the list entirely.

## Support
For help or to report issues, contact the developer or visit the official support page.
