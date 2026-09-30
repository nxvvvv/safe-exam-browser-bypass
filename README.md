# SEB administrator configuration for Windows

This branch provides an example of supported administrator-controlled VM and display settings for an **unmodified** Safe Exam Browser installation. It does not provide a bypass patch for current SEB, conceal the environment, or modify exam logs.

## Version and scope

The target is **SEB for Windows 3.10.2.920**, the latest upstream Windows release checked on **2026-09-30** ([release notes and installers](https://github.com/SafeExamBrowser/seb-win-refactoring/releases/tag/v3.10.2)).

| Item | Status |
| --- | --- |
| Configuration example | Targets the official Windows 3.10.2.920 configuration keys and XML format |
| Operating system | Windows 10 version 1803 or newer, including Windows 11 |
| Runtime dependencies | .NET Framework 4.8 and Visual C++ 2022 Redistributable; use the official setup bundle to install prerequisites |
| macOS and iOS | Outside the scope of this example |
| Root-level EXE and DLL files | Historical **3.6.0.633** patches, unchanged and not supported for the current release |

**Do not copy this repository's `SafeExamBrowser.Client.exe`, `SafeExamBrowser.Monitoring.dll`, or `SafeExamBrowser.SystemComponents.dll` into a current SEB installation.** There is no updated binary patch in this branch. If an existing installation contains modified files, uninstall it through Windows and install a fresh official release before configuring it.

Configuration keys and parsing behavior were checked against upstream commit [`397f8e1`](https://github.com/SafeExamBrowser/seb-win-refactoring/tree/397f8e124387c54a9a770809003dd2e31946dcd5), tagged `v3.10.2`. This is not certification of a particular VM, display driver, or exam platform; no live exam or VM session has been validated here. Recheck the official release notes before deploying a newer release.

## Administrator example

Download [administrator-example.seb](config/administrator-example.seb) and open it **from the SEB Configuration Tool**, using **Open Settings...**. Do not double-click it merely to edit it: Windows normally associates `.seb` files with starting SEB, which can enter kiosk mode.

The file is a readable, unencrypted XML property list for a practice setup, not a protected production exam configuration. It uses SEB's information page as its Start URL and permits quitting without a password.

**This is a standalone example, not an overlay.** Opening it does not merge these settings into an existing exam configuration; omitted settings use the tool's defaults. For an actual exam, open the institution's existing configuration instead and change only the settings approved by the exam administrator.

| Setting | Example value | Meaning |
| --- | --- | --- |
| `startURL` | `https://safeexambrowser.org/start/` | Official information page, not an exam |
| `sebConfigPurpose` | `0` | Start a session; do not replace persistent client settings |
| `allowVirtualMachine` | `true` | Explicitly permit a VM; do not disguise it as a physical machine |
| `allowedDisplaysMaxNumber` | `2` | Allow at most two displays when display detection succeeds |
| `allowedDisplayBuiltinEnforce` | `false` | Do not restrict the setup to built-in displays |
| `allowedDisplaysIgnoreFailure` | `false` | Keep display-validation errors visible and blocking |
| `enableSessionVerification` | `true` | Retain session integrity verification |
| `allowQuit` | `true` | Allow exiting the practice session |

No logging or kiosk-mode overrides are included. Normal monitoring remains active.

**VM permission does not guarantee display compatibility.** The upstream manual warns that display detection can fail inside VMs. This example deliberately leaves **Ignore errors when validating display configuration** off: it will not start successfully on a VM where SEB cannot validate the displays. Use an administrator-approved environment with working display detection rather than treating this example as universal VM support.

## Prepare an approved exam configuration

1. Install [the official Windows setup bundle](https://github.com/SafeExamBrowser/seb-win-refactoring/releases/download/v3.10.2/SEB_3.10.2.920_SetupBundle.exe). Keep every SEB component from the same official installation.
2. Start the installed **SEB Configuration Tool** (`SebWindowsConfig.exe`). Open the practice example for evaluation, or the institution's existing settings for deployment.
3. In **Security**, review **Allow SEB to run inside virtual machine**, **Maximum allowed number of connected displays**, and **Allow only internal displays (laptops)**. Set the actual VM permission and display limit required by the institution; the example's two-display allowance is not an exam policy.
4. In **General**, set the administrator-approved Start URL and administrator and quit/unlock passwords. Review the remaining institution-specific controls, including permitted applications, URL filtering, kiosk mode, and any proctoring requirements.
5. In **Config File**, select **Use SEB settings file for ... starting an exam**, configure encryption with a password or cryptographic identity, and use **Save Settings As...** to create a separate protected `.seb` file. Do not distribute the editable practice file as a production configuration or commit passwords, private keys, or real exam configuration files.
6. Have the exam administrator register the final **Config Key** and/or **Browser Exam Key** with the exam platform, as required. A settings change changes the Config Key; the Browser Exam Key also depends on the SEB build and differs between the x86 and x64 builds. Do not reuse keys from the old patched installation.
7. Run an administrator-supervised practice session on the intended machine, with its intended displays and the final protected settings. Confirm the Start URL, exit procedure, display limit, normal logs, and exam-platform acceptance before deployment.

The exam platform or an institution-managed client can reject configurations it has not approved. This example does not override that authorization.

## Troubleshooting

| Symptom | Supported action |
| --- | --- |
| VM is still rejected | Ask the administrator to confirm that the active, approved configuration permits VMs; editing an unrelated local file does not change the institution's policy |
| Display validation fails | Check the display driver and topology with the administrator or use an approved physical machine; this example does not ignore detection failures |
| Too many displays | Disconnect displays above the approved limit, or have the administrator authorize and redistribute a revised configuration |
| Config Key or Browser Exam Key is rejected | Have the administrator register the keys for the final settings and exact official client build |
| Integrity error after replacing files | Restore a clean official installation; do not mix historical patched files with current components |

Keep SEB's original logs intact when requesting support. The usual Windows log directory is `%LocalAppData%\SafeExamBrowser`.

## References and license

- [Official Windows administrator manual](https://safeexambrowser.org/windows/win_usermanual_en.html)
- [Official SEB downloads](https://safeexambrowser.org/download_en.html)
- [Configuration keys for the pinned release](https://github.com/SafeExamBrowser/seb-win-refactoring/blob/397f8e124387c54a9a770809003dd2e31946dcd5/SafeExamBrowser.Configuration/ConfigurationData/Keys.cs)
- [XML configuration parser for the pinned release](https://github.com/SafeExamBrowser/seb-win-refactoring/blob/397f8e124387c54a9a770809003dd2e31946dcd5/SafeExamBrowser.Configuration/DataFormats/XmlParser.cs)

This repository is not affiliated with the Safe Exam Browser project. Repository contributions are covered by [AGPL-3.0](LICENSE); upstream Safe Exam Browser components retain their own licenses. See the [upstream repository](https://github.com/SafeExamBrowser/seb-win-refactoring) for its source and licensing information.
