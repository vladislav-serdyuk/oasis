# Oasis &nbsp; [![bluebuild build badge](https://github.com/vladislav-serdyuk/oasis/actions/workflows/build.yml/badge.svg)](https://github.com/vladislav-serdyuk/oasis/actions/workflows/build.yml)

A simple, fast, and reliable alternative to Windows. It works with your hardware and runs your favorite apps right out of the box.

![Screenshot](docs/screenshot.png)

## System Requirements

### Minimum
* **Processor:** 64-bit dual-core Intel or AMD CPU
* **RAM:** 4 GB *(Will run on 2 GB configurations automatically thanks to built-in zRAM optimizations)*
* **Storage:** 30 GB of free space *(Oasis occupies only ~21 GB after full setup, including pre-installed apps and Bottles runtimes)*
* **Graphics:** Any GPU with OpenGL 3.2+ support
* **Network:** Standard internet connection

### Recommended
* **Processor:** Quad-core Intel Core i3 / AMD Ryzen 3 or better
* **RAM:** 8 GB *(Forget about memory limits, open 40+ browser tabs with ease)*
* **Storage:** 60 GB+ SSD (Solid State Drive)
* **Graphics:** Any GPU with OpenGL 3.2+ support
* **Network:** High-speed internet connection

## Installation

### Method 1: Fresh Install (Recommended for End Users)
1. Download the latest ISO image: [Download Oasis ISO via Google Drive](https://drive.google.com/drive/folders/1ePK6MGZuuArni343QLJ4MtxyKrbQCp5H?usp=sharing).
2. Flash it to a USB drive using Rufus, Ventoy, or BalenaEtcher.
3. Boot from the USB and follow the on-screen installer.

### Method 2: Rebase Existing Fedora Atomic System

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


## User Guide

### Important Setup Notes
* **Username:** During installation, ensure you set your username using **English characters** only.
* **Keyboard Layout:** Use the **Alt + Shift** shortcut to switch between input languages.
* **Background Installation:** Core applications (such as Firefox, Google Chrome, 
  LibreOffice, etc.) will begin installing automatically in the background 
  once the initial setup is complete.
* **Seamless Background Updates:** No more "Do not turn off your computer" screens. Oasis downloads and prepares updates silently in the background while you work. You can reboot whenever you want — the restart takes only a few seconds, and you can always rollback to the previous working version instantly from the boot menu if anything goes wrong.

### UEFI Secure Boot Support

Oasis is compatible with Secure Boot. However, to use proprietary drivers (like NVIDIA) or virtualization tools (like VirtualBox), it is recommended to add the system's digital signature to your motherboard's trusted list.

*Note: If your motherboard completely blocks the installation media with a **"Secure Boot Violation"** error, you must temporarily disable Secure Boot in your BIOS settings to boot into Oasis and complete the steps below.*

1. Open your terminal in Oasis and run the automatic key enrollment tool:
   ```bash
   ujust enroll-secure-boot-key
   ```
2. Read the instructions on the screen carefully. The script will prepare the certificate and tell you the temporary password (usually `universalblue`).
3. Reboot your laptop:
   ```bash
   systemctl reboot
   ```
4. Upon reboot, a blue textual screen called **MOK Manager** will appear. Don't panic! Follow these quick steps:
  * Select **Enroll MOK** and press Enter.
  * Select **Continue** -> **Yes**.
  * Enter the password: `universalblue` and press **Enter** (Note: characters **will not** be displayed as you type, this is blind input for security).
  * Select **Reboot**.

Once the system starts, your hardware keys are fully synchronized, and all secure modules will load flawlessly!


### Running Windows Apps (.exe) — The First Steps

Oasis configures everything automatically on the first boot. Just follow these simple steps to initialize the environment:

* **Step 1: First-Time Setup** — A system notification will appear, and the **Bottles** app will open automatically. Click **'Not Now'** on the first screen, click the right arrow **`>`** twice, and press **'Continue'**.
* **Step 2: Automatic Initialization** — Wait for the internal setup (13 components) to finish. Once you see the main dashboard, simply **close the Bottles window**. The system will silently create your default `Windows_App` environment in the background and clean up the autostart process.
* **Step 3: Launching & Adding to Start Menu** — Go to any `.exe` file, double-click it, select the pre-configured **`Windows_App`**, and click **'Choose'**. To make the app appear in your Oasis Start Menu:
  1. Open the **Bottles** app.
  2. Select **`Windows_App`** and scroll down to the **"Programs"** section.
  3. Click the **three dots ⋮** next to your app and select **"Add Desktop Entry"**.
  4. Press **Enter** on the system confirmation popup.
  5. Your app is now ready in the **Start Menu**!

### Installing Apps — Using Bazaar
Oasis comes with **Bazaar**, a modern app store that makes installing software as easy as on a smartphone. No need to search for installers on websites!

* **Find Anything:** Open Bazaar from your app menu and search for popular apps like Telegram, Discord, Steam, or Spotify.
* **One-Click Install:** Just click "Install," and the app will be ready to use.
* **Stay Updated:** Bazaar automatically checks for updates for all your installed apps and notifies you when they are ready.

*Note: For the best experience, Oasis uses the **Flatpak** format, which keeps your system fast and secure.*

## Developer & Advanced Information

### Repository Structure

```text
├── .github/workflows/build.yml  # CI/CD pipeline for OCI image generation
├── files/system/etc/            # System configuration overlays
│   ├── fonts/local.conf         # Custom font configurations
│   ├── polkit-1/rules.d/        # Polkit authorization rules (e.g., udisks2)
│   ├── skel/                    # Default user skeleton directory configuration
│   └── systemd/                 # Systemd configuration (zram-generator)
├── files/system/usr/libexec/    # System-level utility scripts (Oasis setup core)
├── modules/                     # Custom build modules
├── recipes/
│   └── recipe.yml               # Main image recipe defining packages and flatpaks
├── cosign.pub                   # Public key for image verification
└── LICENSE                      # Project license
```
### Verification

These images are signed with [Sigstore](https://www.sigstore.dev/)'s [cosign](https://github.com/sigstore/cosign). You can verify the signature by downloading the `cosign.pub` file from this repo and running the following command:

```bash
cosign verify --key cosign.pub ghcr.io/vladislav-serdyuk/oasis
```
