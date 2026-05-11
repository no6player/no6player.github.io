---
title: "How to Fix Shared Printer Error 0x00000bcb on Windows 10 and 11"
date: 2026-05-11
description: "Getting error 0x00000bcb when connecting to a shared printer on Windows 10 or 11? This guide covers all causes and fixes — registry tweak, driver cleanup, and print spooler reset — no third-party tools needed."
tags: ["windows", "error 0x00000bcb", "printer sharing", "registry", "point and print", "printer driver", "windows 10", "windows 11", "troubleshooting"]
categories: ["Troubleshooting"]
cover:
  image: "/images/printer-error-0x00000bcb/step1.png"
  alt: "Windows error dialog showing error 0x00000bcb — The specified printer driver was not found on this system and needs to be downloaded"
  caption: "Error 0x00000bcb appears when Windows cannot install the shared printer driver due to RPC policy restrictions"
  relative: false
---

# How to Fix Shared Printer Error 0x00000bcb on Windows 10 and 11

Error 0x00000bcb is a common shared printer connection failure on Windows 10 and Windows 11. When it appears, Windows displays a message saying the specified printer driver was not found and needs to be downloaded — but the download fails. This article explains exactly why error 0x00000bcb happens and how to fix it step by step.

---

## What Is Error 0x00000bcb?

When you try to connect to a shared printer on your local network, Windows shows:

> **The operation could not be completed (Error 0x00000bcb). The specified printer driver was not found on this system and needs to be downloaded.**

![Error 0x00000bcb dialog on Windows](/images/printer-error-0x00000bcb/step1.png)

Windows automatically tries to install the printer driver from the host PC when you connect to a shared printer. Error 0x00000bcb means this automatic driver installation was blocked — most often by a security policy introduced in recent Windows Updates.

---

## Why Does Error 0x00000bcb Happen?

There are five common causes:

1. **Windows Update tightened RPC security** — Microsoft's security patches restricted Point and Print driver installation to administrators only, blocking automatic driver download from shared printers.
2. **Missing or mismatched printer driver** — The client PC lacks the correct printer driver, or the installed version does not match the one on the host.
3. **Print Spooler service error or corrupted print cache** — A stuck or corrupted print job can prevent new drivers from being installed.
4. **Insufficient LAN permissions** — The client PC does not have the rights needed to pull and install drivers from the host.
5. **Firewall or security software blocking shared communication** — Third-party antivirus or Windows Firewall rules can block printer sharing traffic.

---

## Solution

### Step 1 — Modify the Registry (Point and Print Restriction)

**1.** Press **Win + R**, type `regedit`, and press Enter to open the **Registry Editor**.

![Run dialog with regedit](/images/printer-error-0x00000bcb/step2.png)

**2.** Navigate to the following path. If any part of the path does not exist, create the missing subkeys manually: right-click the parent key → **New → Key**.

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint
```

![Registry Editor — navigating to the PointAndPrint key](/images/printer-error-0x00000bcb/step3.png)

**3.** In the right pane, right-click and select **New → DWORD (32-bit) Value**. Name it `RestrictDriverInstallationToAdministrators`.

![Registry Editor — creating RestrictDriverInstallationToAdministrators DWORD](/images/printer-error-0x00000bcb/step4.png)

**4.** Double-click the new value and set its **Value data** to `0`. Click **OK**.

![Registry Editor — setting RestrictDriverInstallationToAdministrators to 0](/images/printer-error-0x00000bcb/step5.png)

> **What this does:** Setting this value to `0` allows non-administrator users to install printer drivers via Point and Print, which is the mechanism Windows uses to automatically install shared printer drivers.

### Step 2 — Remove the Old Printer Driver

**5.** Open **Control Panel → Hardware and Sound → Devices and Printers**. Click **Print server properties** in the top menu bar, then go to the **Drivers** tab. Select the old driver for the shared printer and click **Remove**.

![Print Server Properties — Drivers tab showing old printer driver](/images/printer-error-0x00000bcb/step6.png)

Removing the stale driver forces Windows to download a clean copy when you reconnect, preventing version conflicts that can also trigger error 0x00000bcb.

### Step 3 — Restart and Reconnect

**6.** Restart your computer. After restarting, try adding the shared printer again. Windows should now be able to download and install the printer driver without showing error 0x00000bcb.

---

## Frequently Asked Questions

**Q: Does error 0x00000bcb affect Windows 10 only, or Windows 11 too?**
Both. Error 0x00000bcb appears on Windows 10 and Windows 11 because the underlying cause — a tightened Point and Print security policy — was applied to both systems via Windows Update.

**Q: Is it safe to set RestrictDriverInstallationToAdministrators to 0?**
Yes, in a home or office LAN environment. This registry value only relaxes the driver installation permission for shared printers on your local network. It does not disable any other security features. If you are in a corporate environment managed by IT, check with your administrator before making registry changes.

**Q: I followed all the steps but still get error 0x00000bcb. What else can I try?**
Try installing the printer driver manually on the client PC first (download it from the manufacturer's website), then connect to the shared printer. This bypasses the automatic driver download entirely and often resolves the issue.

**Q: Only some PCs on the network can't connect — others work fine. Why?**
Computers that fail are usually missing a Windows security patch, have an incompatible driver version, or have a Group Policy setting that overrides the registry fix. Apply the registry change on each affected PC individually.

**Q: Is error 0x00000bcb the same as error 0x00000709?**
They are different error codes but share similar root causes — both involve RPC policy restrictions and driver permission issues. The fixes are also similar. If the steps above do not resolve 0x00000bcb, refer to the [0x00000709 fix guide](/en/posts/printer-sharing-error-0x00000709/) for additional methods.

---

## Summary

Error 0x00000bcb when connecting to a shared printer on Windows 10 or Windows 11 is primarily caused by a Windows Update security policy that restricts automatic printer driver installation. The fix has three steps:

1. Set `RestrictDriverInstallationToAdministrators` = `0` in the registry under `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint`
2. Remove the old printer driver from **Print Server Properties → Drivers**
3. Restart the computer and reconnect to the shared printer

---

## Related Articles

- [How to Fix Shared Printer Error 0x00000709 on Windows](/en/posts/printer-sharing-error-0x00000709/)
- [How to Fix Shared Printer Error 0x0000011b on Windows](/en/posts/printer-sharing-error-0x0000011b/)
- [How to Fix Shared Printer Error 0x000003e3 on Windows 10](/en/posts/printer-sharing-error-0x000003e3/)
