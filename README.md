# Oasis &nbsp; [![bluebuild build badge](https://github.com/vladislav-serdyuk/oasis/actions/workflows/build.yml/badge.svg)](https://github.com/vladislav-serdyuk/oasis/actions/workflows/build.yml)

A simple, fast, and reliable alternative to Windows. It works with your hardware and runs your favorite apps right out of the box.

<img width="1516" height="947" alt="image" src="https://github.com/user-attachments/assets/f16dc100-cd78-40fa-b383-1335bcb6f949" />

## 1. **Important Setup Notes**
* **Username:** During installation, ensure you set your username using **English characters** only.
* **Keyboard Layout:** Use the **Alt + Shift** shortcut to switch between input languages.
* **Background Installation:** Core applications (such as Firefox, Google Chrome, 
  LibreOffice, etc.) will begin installing automatically in the background 
  once the initial setup is complete.


## 2. Running Windows Apps (.exe) — The First Steps
In Oasis, Windows applications run inside protected containers called "Bottles." Since Oasis doesn't have a pre-configured Windows environment to keep the system clean, your first launch requires a few extra steps:

* Step 1: Initial Setup — Double-click your first .exe file. The Bottles app will open and ask for an initial setup. Click "Continue" through the welcome screens until you reach the main dashboard.
* Step 2: Create a Foundation — You cannot run an app "into thin air." Go to the Bottles tab and click the "Create a new Bottle" button.
* Step 3: Configuration — Give it a name (e.g., MyApps) and select the "Application" environment. Wait for the process to finish (the system is creating a virtual C: drive for you).
* Step 4: Launching — Now, go back to your .exe file in the file manager and double-click it again. A window will appear asking which bottle to use. Select the one you just created.
* Step 5: Install — The familiar Windows installer will now start!

Tip: You only need to do steps 1-3 once. For all future apps, just double-click them and select your existing bottle.

## 3. Installing Apps — Using Bazaar
Oasis comes with **Bazaar**, a modern app store that makes installing software as easy as on a smartphone. No need to search for installers on websites!

* **Find Anything:** Open Bazaar from your app menu and search for popular apps like Telegram, Discord, Steam, or Spotify.
* **One-Click Install:** Just click "Install," and the app will be ready to use.
* **Stay Updated:** Bazaar automatically checks for updates for all your installed apps and notifies you when they are ready.

*Note: For the best experience, Oasis uses the **Flatpak** format, which keeps your system fast and secure.*

## Installation

> [!WARNING]  
> [This is an experimental feature](https://www.fedoraproject.org/wiki/Changes/OstreeNativeContainerStable), try at your own discretion.

To rebase an existing atomic Fedora installation to the latest build:

- First rebase to the unsigned image, to get the proper signing keys and policies installed:
  ```
  rpm-ostree rebase ostree-unverified-registry:ghcr.io/vladislav-serdyuk/oasis:latest
  ```
- Reboot to complete the rebase:
  ```
  systemctl reboot
  ```
- Then rebase to the signed image, like so:
  ```
  rpm-ostree rebase ostree-image-signed:docker://ghcr.io/vladislav-serdyuk/oasis:latest
  ```
- Reboot again to complete the installation
  ```
  systemctl reboot
  ```

The `latest` tag will automatically point to the latest build. That build will still always use the Fedora version specified in `recipe.yml`, so you won't get accidentally updated to the next major version.

## ISO
[Download](https://drive.google.com/drive/folders/1ePK6MGZuuArni343QLJ4MtxyKrbQCp5H?usp=sharing)

## Verification

These images are signed with [Sigstore](https://www.sigstore.dev/)'s [cosign](https://github.com/sigstore/cosign). You can verify the signature by downloading the `cosign.pub` file from this repo and running the following command:

```bash
cosign verify --key cosign.pub ghcr.io/vladislav-serdyuk/oasis
```
