# MacBulwark

MacBulwark is a macOS firewall app that puts you back in control of what your apps do on the network. It lets you watch outgoing connections in real time and decide, app by app and domain by domain, what is allowed to reach the internet and what gets dropped.

Once configured, blocking stays in place even when MacBulwark is closed — the rules are enforced at the system level, so you do not need to keep the app running.

## Features

### App-level blocking
Add any application from your `/Applications` folder and choose whether it can talk to the network at all. Each app shows a clear status badge:

- **Blocked** — every outgoing connection is dropped.
- **Allowed** — every connection goes through.
- **Filtered** — a mix of allowed and blocked domains.

A single click on the badge flips an app between states, with a confirmation step before anything destructive happens.

### Per-domain control
Expand any app to see the domains it actually contacts. For each one you can:

- Toggle it on or off individually.
- Edit the hostname or its purpose label.
- Remove it from the list.
- Add new domain rules manually.

This makes it easy to keep an app working for what you need while cutting off the bits you do not want — for example, leaving an app's core service available but blocking its analytics endpoints.

### Purpose labels
MacBulwark tags each domain with a plain-English purpose so you can tell at a glance what the connection is for: **telemetry**, **analytics**, **cdn**, **auth**, **sync**, **account**, **playback**, **metadata**, **update**, or **ai**. Sensitive categories like telemetry and analytics are highlighted so they are easy to spot and shut down.

### Network monitor
A separate Network Monitor view shows recent activity across all your apps — how many connections each one made, how many were blocked, and which hostnames were contacted most recently. From there you can promote any app straight into the firewall list with one click.

### Filter and search
Use the segmented filter in the toolbar to focus on **All**, **Blocked**, **Filtered**, or **Allowed** apps. The status bar at the bottom always shows how many apps are being monitored and how many are currently blocked or filtered.

### Persistent enforcement
Blocking is handled by a system network extension that runs independently of the main window. Quit MacBulwark and your rules stay active. Reboot and they come back automatically.

### Trial mode
MacBulwark runs as a full-featured 24-hour trial out of the box, with no limit on the number of apps you can monitor. After the trial expires, a license key unlocks the app permanently.

### Registration
Licenses are issued through Payhip. Enter your key from the **MacBulwark → Register…** menu and the app unlocks instantly — no restart needed. You can update or unregister the license at any time from the same dialog.

## Getting started

1. Launch MacBulwark.
2. Click **Enable** in the status bar at the bottom of the window.
3. Approve the system extension in System Settings → Privacy & Security.
4. Click the **+** button to add your first app, then click its status pill to start blocking.

That is all there is to it. From that point on, MacBulwark quietly enforces your rules in the background.
