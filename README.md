# ONE-TIME VAULT

**One file. One key. Two locations.**

ONE-TIME VAULT is an experimental, offline desktop application that encrypts files and folders and stores the key material separately on a removable USB drive.

**Created by Masato Nasu — MASATO LAB.**

[Project website](https://masato-nasu.github.io/ONE-TIME-VAULT/) · [MASATO LAB](https://masato-lab.pages.dev/)

> **Status: v0.1.11 public preview.** Linux Mint has been used for hands-on testing. Windows and macOS Python launchers exist, but these platforms are not yet verified in real-world use. This is not an audited cryptographic product.

## The idea

Your encrypted files stay on your computer. Your keys stay on your USB.

1. **Choose a file or folder.** Nested folders are supported.
2. **Encrypt.** The application generates new key material using the operating system's cryptographically secure random source.
3. **Separate.** Encrypted data is saved in a folder on your desktop, while the matching key is written to the selected USB drive.
4. **Decrypt.** Insert the USB drive, choose an encrypted file, and restore it to the desktop. The app automatically searches for the matching key.

No account, server, or cloud upload is required.

## Highlights

- **Files and folders** — Encrypt directory trees, including nested and empty folders.
- **USB key separation** — Encrypted data and its matching key are stored in different places.
- **Automatic key discovery** — Matching keys are identified from internal metadata, not filenames. Both encrypted and key files can be renamed.
- **Recognizable filenames** — The encrypted file normally retains the original name with an `.otv` extension; key files receive random names.
- **Desktop workflow** — Encrypted and decrypted output are placed in separate desktop folders. Originals are not deleted.
- **Offline operation** — File and key data are processed locally.
- **Linux Mint integration** — Desktop application menu, optional system tray, autostart, and a dedicated app icon.
- **Large-file processing** — Streams data rather than loading entire files into memory. Actual performance depends on disk and USB write speeds.

## Install on Linux Mint

**Requirements:** Python 3, Tkinter (`python3-tk`), venv (`python3-venv`), and internet access during initial tray dependency installation.

After downloading the project's **v0.1.11 source package** and extracting it into a folder called `ONE-TIME-VAULT-DESKTOP` under Downloads:

```bash
cd "$(xdg-user-dir DOWNLOAD)/ONE-TIME-VAULT-DESKTOP"
./install-desktop.sh
```

The installer copies the application to `~/.local/share/one-time-vault/`, registers an application menu entry, and configures login autostart for the current user. It does not require root privileges. It installs `pystray` and `Pillow` into a dedicated virtual environment. System tray availability depends on the desktop environment (X11 recommended).

If Tkinter or venv is missing:

```bash
sudo apt install python3-tk python3-venv
```

The uninstall helper removes shortcuts and autostart configuration but intentionally leaves the application data and encrypted files in place.

### Run without installing

```bash
./start-linux.sh
```

On Windows and macOS, the project includes `start-windows.bat` and `start-mac.command` for a suitable Python installation. These are **not** tested native installers. Linux tray/autostart installation is Linux-specific.

## How the files are stored

| Location | Example |
| --- | --- |
| Desktop · Encrypted | `holiday-photos.otv` |
| USB drive | `d74f…e91.otk` |
| Desktop · Decrypted | `holiday-photos/` |

The app searches key files by internal identification and verification information, rather than their names. For reliability, maintain an independent, secure backup of keys.

## How encryption works

The application uses fresh key material generated with an **OS cryptographic random source**; an optional Linux hardware-random source can be mixed in. File data is combined with the keystream by XOR, and **HMAC-SHA256** authenticates the ciphertext and key material.

A new keystream is generated for each encryption. The key is approximately the size of the encrypted payload, so a 1 GB payload needs approximately 1 GB of key storage.

**Important distinction:** Although the project is inspired by one-time-pad encryption, it does **not** claim the information-theoretic security of a mathematically perfect one-time pad. The generated key bytes depend on computer random-number generators; their independence and true randomness cannot be proven in the way strict OTP security requires.

### Limitations and safety

- **Experimental software, not security-audited.** Do not rely on it as the only protection for irreplaceable or highly sensitive information.
- **Key loss means data loss.** Store at least one separate secure key backup. A USB device can fail.
- **FAT32 supports files only up to approximately 4 GB.** Larger key files need a different filesystem, such as exFAT.
- **Original filenames can remain visible** in the encrypted file's external name. Rename the encrypted file if the name itself is sensitive.
- **Some folder entries are skipped.** Symlinks, junctions, and special files are not archived; optional development-folder exclusion also omits corresponding contents. Exclusions are reported in the UI.
- Original files are not automatically securely erased. File metadata, permissions, and timestamps may not be preserved on restoration.
- Do not remove the USB drive until the application reports that writing has fully completed.

## Development

Run tests from the project source directory:

```bash
python3 -m unittest discover -s tests -v
```

The project is written in Python and Tkinter. Version **0.1.11** adds Linux Mint desktop integration and system tray behavior without changing the existing encryption format.

## Project links

- [ONE-TIME VAULT website](https://masato-nasu.github.io/ONE-TIME-VAULT/)
- [MASATO LAB](https://masato-lab.pages.dev/)

---

*ONE-TIME VAULT — offline by design. Experimental cryptography; use with appropriate care.*