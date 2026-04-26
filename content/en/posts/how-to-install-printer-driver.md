---
title: "How to Install a Printer Driver on Windows 10/11"
date: 2026-04-26
description: "A complete step-by-step guide to installing printer drivers on Windows 10 and Windows 11, covering USB, network, and manual installation methods with troubleshooting tips."
tags: ["windows", "driver", "installation", "USB", "network printer"]
categories: ["Installation"]
cover:
  image: "/images/printer-driver/connection-types.svg"
  alt: "Printer connection methods overview"
  caption: "Two main ways to connect a printer: USB and Network/Wi-Fi"
  relative: false
---

Installing a printer driver correctly is the foundation of a working printer setup. This guide walks you through every method — from the simplest plug-and-play to fully manual installation — and shows you how to fix the most common problems.

---

## Before You Start

Before installing, gather the following information:

| What you need | Where to find it |
|---|---|
| Printer model number | Label on the front or bottom of the printer |
| Windows version (10 or 11) | Settings → System → About |
| System type (32-bit / 64-bit) | Settings → System → About → System type |
| Connection type (USB or Wi-Fi) | Decide before you begin |

---

## Method 1 — USB Plug-and-Play (Easiest)

This works for most modern printers right out of the box.

**Steps:**

1. Make sure the printer is **powered off**.
2. Connect the USB cable from the printer to any USB port on your computer.
3. Power the printer **on**.
4. Windows will automatically detect the printer and download the driver.
5. Wait for the notification: *"Device is ready"* in the bottom-right corner.

> **Tip:** Use a USB cable shorter than 3 metres. Longer cables can cause connection errors.

---

## Method 2 — Windows Settings (USB or Network)

Use this method if plug-and-play did not work, or to add a network printer.

![Windows Settings — Printers & scanners panel](/images/printer-driver/windows-settings.svg)

**Steps:**

1. Open **Start** → **Settings** (⚙ icon).
2. Click **Bluetooth & devices** → **Printers & scanners**.
3. Click **Add a printer or scanner** (blue button at the top).
4. Windows will scan for nearby printers. Wait 20–30 seconds.

![Add printer wizard — select your printer from the list](/images/printer-driver/add-printer-wizard.svg)

5. When your printer appears in the list, click it to select it.
6. Click **Add device**.
7. Windows installs the driver automatically. The printer is added to your list when complete.

> **Network printer not appearing?** Make sure the printer and your computer are connected to the **same Wi-Fi network**.

---

## Method 3 — Manual Driver Download

Use this when Windows cannot find the driver automatically, or when you need the full-featured driver with scanning and fax support.

![Manual driver download — four steps](/images/printer-driver/manual-driver-download.svg)

**Steps:**

1. **Find your printer model** — check the label on the printer.
2. **Go to the manufacturer's support website:**
   - HP: `support.hp.com`
   - Canon: `canon.com/support`
   - Epson: `epson.com/support`
   - Brother: `support.brother.com`
3. **Search** for your exact model number (e.g. *LaserJet Pro M404n*).
4. Under **Software and Drivers**, select your operating system.
5. Download the **Full Feature Driver** (recommended) or the Basic Driver.
6. Open the downloaded `.exe` file and follow the on-screen installer.

> **Which driver to download?**
> - **Full Feature Driver** — includes all software (scanner, fax, printer utility). Larger file (~200–400 MB).
> - **Basic Driver** — print only, minimal install. Smaller file (~20–50 MB).

---

## Method 4 — Network Printer via IP Address

Use this when your network printer is not discovered automatically.

**Steps:**

1. Print a **Configuration Page** from the printer's own control panel (usually: *Settings → Reports → Configuration page*). This shows the printer's IP address.
2. Open **Settings** → **Bluetooth & devices** → **Printers & scanners** → **Add a printer or scanner**.
3. Click **"The printer that I want isn't listed"**.
4. Select **"Add a printer using an IP address or hostname"** → **Next**.
5. Enter the printer's IP address (e.g. `192.168.1.105`).
6. Click **Next** — Windows will connect and install the driver.

---

## Troubleshooting

![Common printer problems and solutions](/images/printer-driver/troubleshooting.svg)

### Printer Not Found During Setup

- Check that the printer is **powered on** (not in sleep/power-save mode).
- For USB: try a different USB port; avoid USB hubs.
- For network: confirm both devices are on the same Wi-Fi network.
- Temporarily disable your firewall and retry.

### Printer Shows "Offline"

This is one of the most common issues. To fix it:

1. Press `Win + R`, type `services.msc`, and press **Enter**.
2. Scroll down to **Print Spooler**.
3. Right-click → **Restart**.
4. Open **Devices and Printers**, right-click your printer → **See what's printing**.
5. In the menu, click **Printer** and uncheck **Use Printer Offline**.

### Driver Installation Fails

- Right-click the installer → **Run as administrator**.
- Temporarily pause your antivirus software, then retry.
- Make sure you downloaded the driver for the correct OS version (64-bit vs 32-bit).
- As a last resort, use **Windows Update**: Settings → Windows Update → Check for updates.

### Printer Prints Blank Pages

- Open the printer's software and run the **Print Head Cleaning** utility.
- Check that the ink or toner cartridges are properly seated.
- Make sure you removed all protective tape from new cartridges.

---

## Summary

| Method | Best for | Driver source |
|---|---|---|
| USB Plug-and-Play | New printers, quick setup | Windows Update |
| Windows Settings | USB or network printers | Windows Update |
| Manual download | Full features, failed auto-install | Manufacturer website |
| IP address | Network printers not auto-detected | Windows / Manufacturer |

After installation, always print a **test page**: right-click the printer in Settings → **Printer properties** → **Print a test page**.

If you still have problems after following all methods, check the manufacturer's support website for your specific model — they often publish dedicated troubleshooting guides.
