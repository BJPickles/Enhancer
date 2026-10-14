<div align="center">

<img src="./assets/icon.svg" alt="WandEnhancer icon" width="112" />

# WandEnhancer

**A community-maintained, open-source patcher for the Wand / WeMod desktop client.**

*Client-side Pro-style features · Optional remote trainer control · Reversible local patches*

[![Platform](https://img.shields.io/badge/platform-Windows-0078D4?logo=windows&logoColor=white)](https://github.com/BJPickles/Enhancer)
[![Framework](https://img.shields.io/badge/.NET_Framework-4.8-512BD4?logo=dotnet&logoColor=white)](https://github.com/BJPickles/Enhancer)
[![Build](https://img.shields.io/github/actions/workflow/status/BJPickles/Enhancer/build.yml?branch=master&label=source%20build&logo=githubactions)](./.github/workflows/build.yml)
[![License](https://img.shields.io/badge/license-Apache--2.0-22c55e)](./LICENSE.md)

[**Get started**](#get-started) · [**Features**](#patch-options) · [**Phone remote**](#remote-web-panel) · [**Restore**](#restoring-and-removing-patches) · [**FAQ**](#troubleshooting-and-faq)

</div>

---

> [!IMPORTANT]
> **What this project is, without the marketing language:** WandEnhancer began as **k1tbyte's WeMod Patcher**, designed to remove the original client's daily usage restriction and enable some normally Pro-gated, **client-side** functionality without a paid Pro subscription. This fork continues that local-patching approach for current Wand / WeMod builds. **It does not grant an actual subscription, change your account on the service, or guarantee access to server-controlled features.**
>
> This is an independent community project, **not affiliated with or endorsed by WeMod or Wand**. Client updates can break patches. Review the source and consider the software's terms before using it.

## At a glance

| | |
| :-- | :-- |
| **What changes?** | The installed desktop client's local Electron/JavaScript files and, depending on patch method, the way the app launches or verifies modified files. |
| **What does “Activate WeMod Pro” do?** | Patches client-side account/subscription handling to report an active Pro-style state in the app. It is **not** a real purchase or a server-side entitlement. |
| **What else?** | Optional update blocking, F12 developer tools, a phone-friendly remote panel, custom JavaScript and automatic re-patching after client updates. |
| **Can I undo it?** | Yes, using **Restore** while its original backup files are still present. Reinstalling Wand / WeMod is the fallback if the backup is missing or incompatible. |
| **Does it connect to the internet?** | The standard desktop patch operation does not require telemetry or an update download. **Wand itself is online software**; optional features such as the LAN remote panel and compile-time GitHub release notifications use the network. |
| **How do I get it?** | **Build from source.** This repository does not publish an official prebuilt `.exe` in GitHub Releases. |

## Screenshots

<table>
  <tr>
    <td align="center" width="50%">
      <img src="./assets/screenshots/app1.png" alt="WandEnhancer desktop interface" width="100%" />
      <br /><sub><b>Desktop patcher</b></sub>
    </td>
    <td align="center" width="50%">
      <img src="./assets/screenshots/app2.png" alt="WandEnhancer patch options" width="100%" />
      <br /><sub><b>Patch configuration</b></sub>
    </td>
  </tr>
</table>

<p align="center">
  <a href="./assets/usage-guide.mp4"><b>▶ Watch the short usage demo</b></a>
  &nbsp;·&nbsp;
  <a href="./CHANGELOG.md">Read the changelog</a>
</p>

## Get started

**Recommended route: build the executable on your own GitHub fork, then run it on Windows.** You do not need to install a compiler on your own PC for this route.

### 1. Build using GitHub Actions

1. Open **[BJPickles/Enhancer](https://github.com/BJPickles/Enhancer)** and click **Fork → Create a new fork**.
2. Visit **your fork**, not a third-party download website. Check that its default branch contains the version you want to run.
3. Select the **Actions** tab. If GitHub prompts you, choose to enable workflows for your fork.
4. Open the **Build executable** workflow from the left sidebar.
5. Click **Run workflow** and select **master**. Leave **Include GitHub release checks when Wand starts** unchecked unless you specifically want notifications.
6. Once the run has completed successfully, open that run and find **Artifacts**. Download **WandEnhancer-unsigned**.
7. Extract the downloaded ZIP into a folder you control. The artifact contains `WandEnhancer.exe`. Keep a note of the run URL and its source commit SHA.

<details>
<summary><b>Where is the Run workflow button? What if Actions shows nothing?</b></summary>

- The workflow must exist on your fork's default branch before GitHub can offer manual dispatch.
- In newly created forks, GitHub may require you to explicitly enable workflows.
- The button appears on the selected workflow's page, not the general list of all runs.
- You do **not** need to open a pull request just to build your own fork.
- If you're testing a release candidate, select the exact `feature/rc_*` branch you intend to build. Updating `master` does not automatically update a separate RC branch.

</details>

### 2. Patch your local installation

1. Install **Wand / WeMod** normally and open it once so its application files are present.
2. Close any running game that relies on its trainer, then close the Wand / WeMod desktop app.
3. Run the `WandEnhancer.exe` you built. The patcher normally finds the latest installation automatically. If not, use the folder selector to locate its `app-*` version directory containing `Wand.exe` or `WeMod.exe` and `resources/app.asar`.
4. Click **Enhance** to open **What are we gonna enhance?**
5. Leave **Static (recommended)** selected unless you have a specific reason to use **Supervised (runtime)**.
6. Choose your patch options. **Activate WeMod Pro** is ticked by default; the remote panel, developer tools, update blocking and auto-apply are separate choices.
7. Click **Start**, review any request to close a running Wand instance, and inspect the status log for a success message.
8. Launch Wand / WeMod normally and verify the behaviour you wanted. If it does not work, see [Troubleshooting](#troubleshooting-and-faq).

> [!TIP]
> **Keep the backups.** The patcher creates `app.asar.backup` and `app.asar.unpacked.backup` within the target version's `resources` folder. They make **Restore** possible. Avoid manually deleting or moving them.

> [!CAUTION]
> The static method modifies on-disk application files and can invalidate the modified executable's digital signature. The supervised method modifies execution at runtime and may trigger security software. Neither is guaranteed to work with every client version. Do not dismiss antivirus warnings merely because a patching tool produced them.

## Patch options

| Option in the patcher | What it actually does | Important detail |
| :-- | :-- | :-- |
| **Activate WeMod Pro** | Makes local account-related responses and state appear to contain an active subscription. | Intended to unlock **client-gated** Pro-style behaviour; this does not alter a real account or defeat server-side feature checks. |
| **DevTools on F12** | Adds a developer-tools shortcut in the app. | Useful for debugging; not needed for regular use. |
| **Disable updates** | Patches the client's automatic update behaviour. | May leave you on an outdated version; check for updates yourself. |
| **Enable remote access (beta)** | Injects a local HTTP/WebSocket bridge and a phone-friendly trainer panel. | Other devices can reach the panel over your LAN; control requires a pairing token. |
| **Auto-apply after updates** | Starts a background watcher that attempts to reapply your saved patch choices when a new app version is installed. | **Opt-in persistence:** copies `WandEnhancerWatcher.exe` to the install root and adds a Windows per-user startup entry. |
| **Custom scripts** | Injects JavaScript files you select into the app renderer. | Scripts have extensive access to the client; never add untrusted files. |

### Which patch method?

| | Static (recommended) | Supervised (runtime) |
| :-- | :-- | :-- |
| How it works | Applies changes to installed files | Applies necessary changes as the application launches |
| Digital signature | Changes to the executable can invalidate its signature | May avoid the same on-disk executable modification |
| Trade-off | Sensitive to client integrity checks and updates | Launch-time instrumentation can trip antivirus / compatibility checks |
| Best for | Most users when supported | Advanced troubleshooting or testing |

**The Pro wording matters:** k1tbyte's [original 2024 README](https://github.com/k1tbyte/Wand-Enhancer/blob/c6ca06e6dd4a69f6c9485b0195b80246cf05cd15/README.md) described removing a two-hour daily limit and enabling selected Pro features, rather than selling or assigning a real subscription. The current implementation modifies the client's account state; results vary with client versions, individual features, and what the service checks remotely.

## Remote Web Panel

Turn on **Enable remote access (beta)** when patching if you want to adjust supported trainer controls from a phone or tablet.

### Connect your phone

1. Connect the PC and phone to the **same trusted local network**. A PC on Ethernet and phone on Wi-Fi is fine if they can reach each other.
2. Start the **patched Wand / WeMod app** and bring up the remote/Connect control.
3. Open its QR code or advertised pairing link, then scan/open it on your phone.
4. The URL includes a **pairing token**, and the phone sends it when opening the WebSocket session. Once connected, the panel can show trainer state and provide controls for supported actions.
5. Bookmark or revisit the panel only on a trusted device. Pairing tokens rotate when the bridge restarts, so rescan the current QR code after a new session if necessary.

**Networking and safety**

- The bridge normally starts on TCP port **3223**, but can choose another nearby port if that one is unavailable. Use the actual address displayed in the app.
- It serves a page using **plain HTTP** on your local network; WebSocket control uses a token, but the connection is **not end-to-end TLS**. Only use trusted networks or a private VPN.
- **Do not port-forward the bridge to the public internet.** For access from elsewhere, use a private VPN such as [Tailscale](https://tailscale.com/), not an open router port.
- Treat the QR code and URLs containing `?token=` as sensitive: anyone who obtains a valid token and can reach the bridge may control the active trainer.
- The unauthenticated health endpoint exposes some operational status, so pairing is **not** a reason to place the service on an untrusted LAN.

<details>
<summary><b>Phone cannot connect?</b></summary>

1. Confirm that the remote option was enabled **before** patching and that the patched desktop app is running.
2. Scan a **fresh QR code** rather than typing an old URL; the pairing token changes between bridge starts.
3. Check for guest-Wi-Fi isolation / AP isolation, which can block devices from reaching one another.
4. Check Windows Firewall for the actual bridge port. Restrict any necessary allowance to your **Private** network, not a public network.
5. If using a VPN, verify both devices can reach the PC's VPN address. Do not expose the bridge publicly just to make the QR code work.

</details>

## Custom JavaScript

WandEnhancer can bundle your own `.js` files into the client renderer. **Enable remote access (beta)** as well, because the current custom-script injection uses the same renderer-injection mechanism.

There are two supported ways to provide scripts:

1. Click **Add .js** in the patch options and select one or more local JavaScript files.
2. Place files in a `renderer-scripts/` directory next to `WandEnhancer.exe` before patching.

For example, a harmless visual change saved as `hello.js`:

```js
// Scripts may run more than once per app launch.
if (!globalThis.__wandEnhancerHello) {
  globalThis.__wandEnhancerHello = true;
  console.log("[WandEnhancer] Custom renderer script loaded.");

  document.documentElement.dataset.wandEnhancerExample = "active";
}
```

The script will be bundled into the patched renderer. Custom scripts have access to the document and can use Node `require` in this environment. That means **a malicious script could read local application data**. Do not run JavaScript copied from strangers without inspecting it.

## Build on your own PC

If you prefer not to use GitHub Actions, compile the project locally on **Windows**.

**Prerequisites**

- **Node.js 22** and **pnpm 10.17.0** (used by CI and the lockfile).
- **Visual Studio 2022** or **Visual Studio Build Tools 2022**, with MSBuild.
- **.NET Framework 4.8 targeting / desktop build tools**.
- Git (or a ZIP download of the source).

Open PowerShell:

```powershell
git clone https://github.com/BJPickles/Enhancer.git
cd Enhancer
node --version
pnpm --version
.\build.cmd
```

On success, the compiled application is at:

```text
WandEnhancer\bin\Release\WandEnhancer.exe
```

The build script installs dependencies from the lockfile, runs lint and type checks, builds the web panel, runs web tests, restores .NET dependencies, builds the Windows application, and runs desktop patch-related checks. **Build tests do not patch your installed Wand app.**

**Optional GitHub release notifications** are excluded from the normal build. To compile the notifier into a local build:

```powershell
.\build.cmd -EnableUpdateNotifications
```

That feature checks GitHub for published releases; it does **not** download or automatically install them. It is different from the **Auto-apply after updates** checkbox, which watches updates of the installed Wand / WeMod application.

## Restoring and removing patches

To undo changes to an installation:

1. Close Wand / WeMod and any affected game first.
2. Launch `WandEnhancer.exe` and select the **same installed version** that was patched.
3. Click **Restore**. Confirm any prompt to close a still-running client.
4. Verify a successful restore message before reopening the app.

**Restore relies on intact backups for that specific app version.** If the backups are missing, incomplete, or no longer match the installation, reinstall/repair the original application rather than copying arbitrary files from the internet.

> [!WARNING]
> **Auto-apply cleanup:** If you previously ticked **Auto-apply after updates**, the code registers a `WandEnhancerWatcher_<Brand>` value under `HKCU\Software\Microsoft\Windows\CurrentVersion\Run` and starts `WandEnhancerWatcher.exe`. Unchecking auto-apply during a successful re-patch invokes the watcher's stop/unregister logic. **The current Restore implementation removes the saved patch configuration but does not explicitly unregister that startup entry.** If you used auto-apply and are removing the tool, check Windows Startup apps / the per-user Run entry and remove any leftover watcher entry as appropriate. This is not a network update agent; it monitors the application install directory to reapply saved patches.

## Troubleshooting and FAQ

<details>
<summary><b>The patcher can't find Wand / WeMod</b></summary>

Install and launch the desktop client once. The patcher checks the usual `%LOCALAPPDATA%\Wand` and `%LOCALAPPDATA%\WeMod` roots for `app-*` version folders. You can select the matching version folder manually. The folder must contain the application executable and `resources\app.asar`.

</details>

<details>
<summary><b>It says Access denied, or cannot patch / restore</b></summary>

Close the client and any running game using its trainer, then retry. The patcher can ask before force-closing the app. A DLL or other process may still hold a file open even after the app appears closed. Avoid manually editing partial backups; if restoration fails, use the original app installer to repair the installation.

</details>

<details>
<summary><b>A client update broke trainer loading or Pro-style behaviour</b></summary>

New app versions can rename or restructure client code and integrity checks. Record your **exact Wand / WeMod version**, build commit SHA, patch method and chosen features. Try Restore first, then use an updated, reviewed build when available. Disabling updates can improve short-term compatibility but comes with the risk of running outdated software.

</details>

<details>
<summary><b>Why did Windows Defender / SmartScreen flag the executable?</b></summary>

The application is unsigned and modifies another application's installed files and/or runtime behaviour. Those patterns can be flagged by security products. **That does not prove a false positive.** Do not disable antivirus globally or run random third-party installers; inspect the code, verify the GitHub Actions run that built your file and scan the resulting executable.

</details>

<details>
<summary><b>Is the application offline? Does it transmit an account token?</b></summary>

The standard desktop patch process operates on local files and has no built-in telemetry upload. The host app is network-connected; the optional remote panel opens a LAN HTTP/WebSocket listener; and release checks are an explicit compile-time option. The remote protocol is designed around trainer state and commands, not sending Wand account bearer tokens to the phone, but use only trusted builds and devices.

</details>

<details>
<summary><b>Where do I report a problem?</b></summary>

Use [Issues](https://github.com/BJPickles/Enhancer/issues) and include the Wand / WeMod version, your Enhancer build commit, selected patch method and options, and exact error text. For startup failures, `launcher.log` and `launcher.prev.log` may help; **redact usernames, paths, pairing tokens and any other secrets** before sharing. Never attach account storage dumps or unknown executables.

</details>

## Security, trust and scope

- **Source-first distribution:** this repository does not publish a trusted ready-made `.exe` release. Use your own fork's Actions artifact or a local build.
- **No official video-download mirrors:** tutorials, Discord attachments, advertising links and third-party executables may be unrelated or malicious. A video using this project's name is not proof of authenticity.
- **Local modification, not an account upgrade:** a client reporting “Pro” does not mean an online account or server entitlement has been changed.
- **Back up and verify:** keep your original app installer and patch backups; review changes before building, especially changes to `.github/workflows/`, installer scripts, custom scripts or background services.
- **Use responsibly:** modifying a third-party client may affect stability, warranty, terms of service, or account access. The project makes no guarantee that patches will work with every version.

## Origins and credits

WandEnhancer continues the work of **[k1tbyte (Killbyte)](https://github.com/k1tbyte)**, whose original **WeMod Patcher** explicitly focused on enabling selected Pro features locally, including removing the daily time limit. The project later evolved into a Windows desktop patcher and was renamed **WandEnhancer**.

- [Original project and history](https://github.com/k1tbyte/Wand-Enhancer)
- [Original 2024 README](https://github.com/k1tbyte/Wand-Enhancer/blob/c6ca06e6dd4a69f6c9485b0195b80246cf05cd15/README.md)
- [This fork's change history](./CHANGELOG.md)

This repository is independently maintained. Attribution acknowledges the original author and contributors; it does not imply their endorsement of every change in this fork.

## License

Licensed under the [Apache License 2.0](./LICENSE.md). The licence covers this project's own source, **not** third-party application binaries or services.

---

<div align="center">

**Know what you are patching. Build from source. Keep your backups.**

[Back to top ↑](#wandenhancer)

</div>
