# Helium Flatpak

This repository contains the [Flatpak](https://flatpak.org/) manifest for **Helium**, a private, fast, and honest web browser based on Ungoogled Chromium.

It wraps the official prebuilt binaries from the [Helium Linux project](https://github.com/imputnet/helium-linux) into a sandboxed Flatpak environment,
ensuring it runs consistently across different Linux distributions.  
Sandbox protection is supported via Zypak. The signed Flatpak repository provides automatic updates.

---

## Installation (Recommended)

Add the signed Helium repository and install the application:

```bash
flatpak remote-add --if-not-exists helium https://helium-flatpak.nhaiden.io/helium.flatpakrepo
flatpak install helium net.imput.helium
```

Flatpak will deliver future Helium updates from this repository.

### Standalone Bundle

Standalone `.flatpak` bundles remain available from the [Releases Page](https://github.com/NiHaiden/helium-flatpak/releases). Download the bundle for your architecture and install it with:

```bash
flatpak install ./helium-[VERSION]-[ARCH].flatpak
```

On some distributions, you can also open the downloaded file in your Software Center.

---

## Building from Source

If you want to build the package yourself or contribute to the manifest, follow these steps.

### Prerequisites
Ensure you have `flatpak` and `flatpak-builder` installed. You also need the Flathub repository enabled to download the Freedesktop SDK/Runtime (version 24.08).

```bash
flatpak remote-add --user --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
flatpak install org.freedesktop.Sdk/x86_64/24.08
```

### Build & Install
Run the following command in the root of this repository. This will download the binary, build the sandbox, and install it to your user directory.

For x86_64 systems:

```bash
flatpak-builder --arch=x86_64 --user --install --force-clean build-dir net.imput.helium.yml
```

For ARM64 systems:

```bash
flatpak-builder --arch=aarch64 --user --install --force-clean build-dir net.imput.helium.yml
```

*Note: to install for all users, use sudo and replace '--user' with '--system'.*

---

## Repository Publishing

The public Cloudflare R2 bucket endpoint is [https://helium-flatpak.nhaiden.io/](https://helium-flatpak.nhaiden.io/). The repository descriptor is available at [`helium.flatpakrepo`](https://helium-flatpak.nhaiden.io/helium.flatpakrepo).

The `Helium Auto Update` workflow continues to attach standalone bundles to GitHub Releases and also publishes a signed, multi-architecture OSTree repository to Cloudflare R2. It expects these GitHub Actions secrets:

- `FLATPAK_GPG_PRIVATE_KEY_B64`
- `FLATPAK_GPG_KEY_ID`
- `R2_ACCOUNT_ID`
- `R2_ACCESS_KEY_ID`
- `R2_SECRET_ACCESS_KEY`
- `R2_BUCKET`

The publish job uses the `flatpak-repository` GitHub environment. A manual workflow run rebuilds and publishes the current version without creating a duplicate GitHub Release.

---

## Running the App

Once installed (via bundle or local build), you can launch Helium from your application menu or via the terminal:

```bash
flatpak run net.imput.helium
```

---

## Uninstallation

To remove Helium and its data:

```bash
flatpak uninstall net.imput.helium
# Optional: Remove app data
rm -rf ~/.var/app/net.imput.helium
```

---

**Disclaimer:** This is an unofficial packaging project. For issues related to the browser itself, please refer to the [upstream repository](https://github.com/imputnet/helium). For packaging issues, feel free to open an issue here.
