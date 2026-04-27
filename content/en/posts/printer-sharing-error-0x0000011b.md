---
title: "How to Fix Printer Sharing Error 0x0000011b on Windows"
date: 2026-04-27
description: "Getting the 'Windows cannot connect to the printer, error 0x0000011b' message? This guide walks you through the proven registry fix to restore shared printer connections instantly."
tags: ["windows", "error 0x0000011b", "printer sharing", "registry", "troubleshooting"]
categories: ["Troubleshooting"]
cover:
  image: "/images/printer-error-0x0000011b/step1.png"
  alt: "Windows error dialog showing operation failed with error 0x0000011b"
  caption: "The 0x0000011b error appears when Windows cannot connect to a shared printer"
  relative: false
---

Shared printers are a cornerstone of productive office and home-lab setups — one printer, many users, no extra hardware. Yet a frustrating error has been stopping many Windows users in their tracks. When trying to connect to a shared printer on the local network, Windows pops up the message:

> **"Windows cannot connect to the printer. Operation failed with error 0x0000011b."**

![Error 0x0000011b dialog on Windows](/images/printer-error-0x0000011b/step1.png)

This error became widespread after a Microsoft security update (KB5005565, released in late 2021) introduced stricter RPC authentication enforcement. While the patch is important for security, it inadvertently broke shared-printer connectivity for countless users across Windows 10 and Windows 11.

The good news: the fix takes less than two minutes and requires no third-party software.

---

## Why Does Error 0x0000011b Happen?

The root cause is a change in how Windows handles **RPC (Remote Procedure Call) authentication privacy** when communicating with shared printers. After the security update, Windows started requiring a higher authentication level by default — and older or differently configured systems on the same network do not always negotiate it correctly, causing the connection to fail with error `0x0000011b`.

---

## Fix — Edit the Windows Registry

This solution disables the strict RPC authentication requirement on the **client** computer (the one trying to connect to the shared printer), which is safe for most home and office networks.

> ⚠️ **Before you begin:** Editing the registry incorrectly can cause system problems. Follow the steps exactly as written. If you are unsure, create a registry backup first: in Registry Editor, go to **File → Export** and save a full backup.

### Step 1 — Open Registry Editor

Press **Win + R** to open the Run dialog (or search in the Start menu), type `regedit`, and press **Enter**. Click **Yes** if prompted by User Account Control.

![Search for regedit in the Windows Start menu](/images/printer-error-0x0000011b/step2.png)

### Step 2 — Navigate to the Print Key

In the address bar at the top of Registry Editor, paste the following path and press **Enter**:

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print
```

![Registry Editor navigated to the Control\Print key](/images/printer-error-0x0000011b/step3.png)

### Step 3 — Create a New DWORD Value

1. Right-click on an empty area in the right-hand pane.
2. Select **New → DWORD (32-bit) Value**.

![Right-click menu to create a new DWORD (32-bit) value](/images/printer-error-0x0000011b/step4.png)

3. Name the new value exactly:

```
RpcAuthnLevelPrivacyEnabled
```

### Step 4 — Set the Value to 0

1. Double-click the newly created `RpcAuthnLevelPrivacyEnabled` entry.
2. In the **Value data** field, enter `0`.
3. Make sure **Base** is set to **Hexadecimal**.
4. Click **OK**.

![Setting RpcAuthnLevelPrivacyEnabled to 0 in the Edit DWORD dialog](/images/printer-error-0x0000011b/step5.png)

### Step 5 — Restart and Verify

Close Registry Editor and **restart your computer**. After rebooting, try connecting to the shared printer again — the connection should succeed.

![Printer queue showing successful connection after the fix](/images/printer-error-0x0000011b/step6.png)

---

## Why This Fix Works

Setting `RpcAuthnLevelPrivacyEnabled` to `0` tells Windows to use the previous, less strict RPC authentication behaviour when communicating with shared printers. This restores compatibility with print servers that haven't been updated or configured to support the newer authentication requirement.

---

## Still Getting the Error?

If the fix above did not resolve your issue, check these additional steps:

- **Try the fix on the host computer too** — apply the same registry change on the PC that is sharing the printer, then restart it.
- **Check the Windows Firewall** — make sure File and Printer Sharing is allowed through the firewall on both computers.
- **Verify both computers are on the same network** — confirm they share the same subnet (e.g., both have IP addresses starting with `192.168.1.x`).
- **Restart the Print Spooler service** — press Win + R, type `services.msc`, find **Print Spooler**, right-click and select **Restart**.

---

## Summary

| Step | Action |
|---|---|
| 1 | Open Registry Editor (`Win + R` → `regedit`) |
| 2 | Navigate to `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print` |
| 3 | Create a new **DWORD (32-bit)** value |
| 4 | Name it `RpcAuthnLevelPrivacyEnabled` and set it to `0` |
| 5 | Restart your computer |

Error 0x0000011b is annoying but straightforward to fix once you know the cause. The registry edit takes under two minutes and reliably restores shared printer access on Windows 10 and Windows 11.
