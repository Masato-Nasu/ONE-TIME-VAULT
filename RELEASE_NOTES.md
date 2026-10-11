# ONE-TIME VAULT v0.1.12

An experimental offline encryption tool for Linux Mint with a bilingual Japanese/English interface.

**Download:** Choose `ONE-TIME-VAULT-DESKTOP-v0.1.12.zip` under **Assets** below. This package includes the Python/Tkinter desktop application, installers, source code, and tests.

**Linux Mint installation**

```bash
cd "$(xdg-user-dir DOWNLOAD)"
unzip -oq ONE-TIME-VAULT-DESKTOP-v0.1.12.zip
cd ONE-TIME-VAULT-DESKTOP
./install-desktop.sh
```

Version 0.1.12 adds a Japanese / English language switch, persisted between sessions. Encrypted data stays on the computer and the matching key is stored separately on a USB drive. Existing `.otv` and `.otk` formats remain compatible.

**Note:** This is an experimental cryptography application and has not undergone independent security review. Linux Mint is the tested environment; Windows and macOS are not yet verified. Always back up keys securely.

[Read the README](https://github.com/Masato-Nasu/ONE-TIME-VAULT#readme) · [Project website](https://masato-nasu.github.io/ONE-TIME-VAULT/)