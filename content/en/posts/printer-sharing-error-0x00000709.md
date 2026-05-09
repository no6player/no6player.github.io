---
title: "How to Fix Shared Printer Connection Error 0x00000709 on Windows"
date: 2026-04-29
description: "Getting error 0x00000709 when connecting to a shared printer on Windows? This guide walks you through fixing it by adding Windows credentials and modifying a registry value."
tags: ["windows", "error 0x00000709", "printer sharing", "credential manager", "registry", "troubleshooting"]
categories: ["Troubleshooting"]
cover:
  image: "/images/printer-error-0x00000709/step1.png"
  alt: "Windows error dialog showing error 0x00000709 — Operation could not be completed"
  caption: "Error 0x00000709 appears when Windows cannot connect to a shared printer on the network"
  relative: false
---

## Problem Description

When trying to connect to a shared printer on the network, the connection fails with the following error:

> **The operation could not be completed (Error 0x00000709). Double-check the printer name and make sure the printer is connected to the network.**

![Error 0x00000709 dialog on Windows](/images/printer-error-0x00000709/step1.png)

Error 0x00000709 is a common shared printer connection error on Windows. It is typically caused by a system update, an RPC protocol misconfiguration, or a credential authentication issue. The fix involves adding Windows credentials and modifying a registry value.

---

## Solution

**1.** Open **Control Panel**, set the view to **Small icons**, then click **Credential Manager**.

![Control Panel — Credential Manager](/images/printer-error-0x00000709/step2.png)

**2.** Select **Windows Credentials**, then click **Add a Windows credential**.

![Credential Manager — Add a Windows credential](/images/printer-error-0x00000709/step3.png)

**3.** In the **Internet or network address** field, enter the host PC's IP address. Set the **User name** to `guest` and leave the **Password** field empty. Click **OK**.

![Add Windows Credential dialog — enter host IP and guest username](/images/printer-error-0x00000709/step4.png)

**4.** Press **Win + R**, type `regedit`, and press Enter to open the **Registry Editor**.

![Run dialog with regedit](/images/printer-error-0x00000709/step5.png)

**5.** Navigate to the following key (create it if it does not exist):

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC
```

Right-click in the right pane and create a new **DWORD (32-bit) Value**.

![Registry Editor — creating a new DWORD value under the RPC key](/images/printer-error-0x00000709/step6.png)

**6.** Rename the new value to `RpcUseNamedPipeProtocol` and set its **Value data** to `1`.

![Registry Editor — RpcUseNamedPipeProtocol set to 1](/images/printer-error-0x00000709/step7.png)

**7.** Once all steps are complete, **restart your computer** and try connecting to the shared printer again.

---

## Summary

Error 0x00000709 when connecting to a shared printer is a common issue on Windows, usually caused by a system update patch, an RPC protocol configuration problem, or a credential authentication failure. The fix involves two steps:

- Adding the host PC's IP address and the **guest** account to **Windows Credential Manager**
- Creating the registry value **RpcUseNamedPipeProtocol** (`1`) under `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC`

After restarting, the shared printer connection should work normally.
