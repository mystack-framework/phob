# PHOB — PHP High-Performance Obfuscator & Licensing Guard (v4.0.0-hardened)

[![PHP Version](https://img.shields.io/badge/PHP-8.1%20%7C%208.2%20%7C%208.3%20%7C%208.4%2B-blue.svg)](https://www.php.net)
[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20macOS-brightgreen.svg)](#-system-requirements)
[![Repository](https://img.shields.io/badge/GitHub-mystack--framework%2Fphob-blue?logo=github)](https://github.com/mystack-framework/phob)

**PHOB** is an enterprise-grade, high-security C extension for PHP (supporting PHP 8.1, 8.2, 8.3, and 8.4+). It provides multi-layer source code encryption, byte-level obfuscation, hardware-locked device authentication, tamper protection, and in-memory execution designed to protect proprietary PHP intellectual property from reverse engineering, decompilation, and unauthorized redistribution.

---

## 📑 Table of Contents
1. [Core Features](#-core-features)
2. [Security Architecture & Anti-Tamper Protections](#-security-architecture--anti-tamper-protections)
3. [Binary Downloads](#-binary-downloads)
4. [System Requirements](#-system-requirements)
5. [Complete Installation Guide (Windows, Linux, macOS)](#-complete-installation-guide)
   - [Windows Installation (WAMP / XAMPP / IIS / CLI)](#1-windows-installation)
   - [Linux Installation (Ubuntu / Debian / RHEL / CentOS)](#2-linux-installation)
   - [macOS Installation (Homebrew / MAMP / Native)](#3-macos-installation)
6. [API Reference & Usage](#-api-reference--usage)
   - [`phob_deviceID()`](#1-phob_deviceid)
   - [`phob_build()`](#2-phob_build)
   - [`phob_use()`](#3-phob_use)
7. [Complete Step-by-Step Example](#-complete-step-by-step-example)
   - [Step 1: Check Target Machine Device ID](#step-1-get-target-hardware-id)
   - [Step 2: Obfuscate & Encrypt Application Code](#step-2-build-encrypted-package)
   - [Step 3: Execute Protected Code Securely](#step-3-run-protected-code)
8. [Licensing Rules & Enforcement Matrix](#-licensing-rules--enforcement-matrix)
9. [Troubleshooting & FAQ](#-troubleshooting--faq)
10. [Security Best Practices](#-security-best-practices)

---

## 🚀 Core Features

- **Multi-Layer AES-256-GCM Encryption**: High-security symmetric authenticated encryption with unique initialization vectors (IVs) and authentication tags for every encrypted file.
- **Dynamic Hex-Noise Injection (Puzzle Defense)**: Injects pseudo-random bytes and nonces into ciphertext chunks to defeat automated static analysis and signature scanning.
- **Hardware-Level Device Locking**: Cryptographically binds encrypted scripts to unique machine hardware signatures (Motherboard/DMI UUID on Linux, MachineGuid on Windows, IOPlatformUUID on macOS).
- **Time-Bound Licensing**: Enforces granular expiry dates (`DD/MM/YYYY`) embedded and signed inside the encrypted envelope.
- **Pure In-Memory Decryption & Execution**: Decrypts directly into volatile memory; plain source code is never written to temporary files or persistent disk storage.
- **Memory Zero-Wiping (`memset`)**: Decrypted memory buffers are scrubbed immediately after compilation into Zend opcodes.
- **Zero External Dependencies**: Operates with native system crypto (OpenSSL) and direct kernel/registry calls without third-party shell dependencies.

---

## 🛡️ Security Architecture & Anti-Tamper Protections

| Defense Layer | Mechanism | Protection Scope |
| :--- | :--- | :--- |
| **White-Box Secret Synthesis** | Algorithmic XOR runtime key derivation | Prevents static extraction of encryption keys via `strings` or hex editors |
| **Anti-Debugging (Linux)** | `ptrace(PTRACE_TRACEME)` | Immediately terminates process if GDB, LLDB, or Strace attach to memory |
| **Anti-Debugging (Windows)** | `IsDebuggerPresent()` | Prohibits attachment of user-mode debuggers (e.g. x64dbg) |
| **Anti-Dump Protection** | `prctl(PR_SET_DUMPABLE, 0)` | Prevents generating core dumps or reading `/proc/[pid]/mem` on Linux |
| **Zend Compiler Guard** | Pointer snapshotting (`orig_compile_string`) | Prevents malicious third-party PHP extensions from hooking bytecode compilation |
| **Symbol Stripping** | Stripped ELF/PE symbol tables (`-s`, `strip --strip-all`) | Removes internal function labels, making binary disassembly opaque |

---

## ⬇️ Binary Downloads

Download the pre-compiled, stripped binaries directly from the repository:

| Platform | Binary Name | Download URL |
| :--- | :--- | :--- |
| **Windows (x64)** | `phob.dll` | [https://github.com/mystack-framework/phob/blob/main/phob.dll](https://github.com/mystack-framework/phob/blob/main/phob.dll) |
| **Linux (x86_64)** | `phob.so` | [https://github.com/mystack-framework/phob/blob/main/phob.so](https://github.com/mystack-framework/phob/blob/main/phob.so) |

---

## 💻 System Requirements

- **PHP Version**: PHP 8.1, 8.2, 8.3, or 8.4+
- **Architectures**: x86_64 / amd64 / Apple Silicon (ARM64)
- **Supported Operating Systems**:
  - **Windows**: Windows 10, Windows 11, Windows Server 2016+ (PHP x64)
  - **Linux**: Ubuntu 20.04+, Debian 11+, RHEL 8+, CentOS, Rocky Linux, Alpine
  - **macOS**: macOS Monterey (12+), Ventura (13+), Sonoma (14+), Sequoia (15+)

---

## 📦 Complete Installation Guide

### 1. Windows Installation
Supported on WampServer, XAMPP, IIS, and Standalone PHP CLI.

#### Step 1: Download `phob.dll`
Download [`phob.dll`](https://github.com/mystack-framework/phob/blob/main/phob.dll) and place it into your active PHP `ext/` directory.

- **Check your PHP extension directory:**
  ```powershell
  php -r "echo ini_get('extension_dir');"
  ```
  *(Example path: `C:\wamp64\bin\php\php8.3.28\ext\` or `C:\tools\php83\ext\`)*

- **Copy command:**
  ```powershell
  Copy-Item phob.dll (php -r "echo ini_get('extension_dir');")\phob.dll
  ```

#### Step 2: Enable in `php.ini`
Open your active `php.ini` file (locate it with `php --ini`) and append:
```ini
extension=phob.dll
```

#### Step 3: Restart Web Server
- For **WAMP / XAMPP**: Restart Apache / PHP-FPM service from the system tray.
- For **IIS**: Run `iisreset` in elevated Command Prompt / PowerShell.

#### Step 4: Verify
```powershell
php --ri phob
```
*Expected Output:*
```text
phob
phob support => enabled
Version => 4.0.0-hardened
License Key Check => Supported
Device ID Check => Supported
Expiry Date Check => Supported
```

---

### 2. Linux Installation
Supported on Ubuntu, Debian, CentOS, RHEL, AlmaLinux, and Rocky Linux.

#### Step 1: Download `phob.so`
Download [`phob.so`](https://github.com/mystack-framework/phob/blob/main/phob.so) directly using `curl` or `wget`:

```bash
# Locate extension directory
EXT_DIR=$(php -r "echo ini_get('extension_dir');")

# Download directly into the extension directory
sudo curl -sSL -o "${EXT_DIR}/phob.so" "https://raw.githubusercontent.com/mystack-framework/phob/main/phob.so"

# Grant appropriate read & execute permissions
sudo chmod 755 "${EXT_DIR}/phob.so"
```

#### Step 2: Enable Extension in PHP
Create a configuration ini file or edit `php.ini`:

- **For Ubuntu / Debian (CLI & FPM):**
  ```bash
  echo "extension=phob.so" | sudo tee /etc/php/8.3/mods-available/phob.ini
  sudo phpenmod phob
  ```

- **For RHEL / CentOS / AlmaLinux / Direct `php.ini`:**
  ```bash
  echo "extension=phob.so" | sudo tee -a /etc/php.ini
  ```

#### Step 3: Restart Web Services
```bash
# If using PHP-FPM:
sudo systemctl restart php8.3-fpm

# If using Apache:
sudo systemctl restart apache2
# or on CentOS/RHEL:
sudo systemctl restart httpd

# If using Nginx:
sudo systemctl restart nginx
```

#### Step 4: Verify
```bash
php --ri phob
```

---

### 3. macOS Installation
Supported on Homebrew PHP, MAMP, and standalone builds (Intel x86_64 & Apple Silicon ARM64).

#### Step 1: Place `phob.so` in Extension Directory
```bash
EXT_DIR=$(php -r "echo ini_get('extension_dir');")
sudo cp phob.so "${EXT_DIR}/phob.so"
sudo chmod 755 "${EXT_DIR}/phob.so"
```

#### Step 2: Configure `php.ini`
Find your configuration file:
```bash
php --ini
```
Edit your `php.ini` (e.g., `/opt/homebrew/etc/php/8.3/php.ini` or `/usr/local/etc/php/8.3/php.ini`) and add:
```ini
extension=phob.so
```

#### Step 3: Restart Web Services
- If using Homebrew service:
  ```bash
  brew services restart php
  ```
- If using Valet:
  ```bash
  valet restart
  ```
- If using MAMP: Restart servers from the MAMP control panel.

#### Step 4: Verify
```bash
php --ri phob
```

---

## 📖 API Reference & Usage

The PHOB extension exposes three core functions:

### 1. `phob_deviceID()`
Returns the unique hardware identifier of the machine currently executing PHP.

```php
string phob_deviceID(void)
```
- **Returns**: `string` — A 14-character alphanumeric uppercase hardware hash (e.g., `VRJYS7F6RHGER4`).
- **Use Case**: Run this function on the client's machine to obtain their target Device ID before issuing a locked license.

---

### 2. `phob_build()`
Obfuscates, encrypts, and packages a raw PHP source file into a protected distribution file.

```php
bool phob_build(
    string $input_path, 
    string $output_path, 
    array $skip_list = [], 
    array $config = []
)
```

#### Parameters:
- **`$input_path`** *(string)*: Absolute or relative path to the plain PHP script to protect.
- **`$output_path`** *(string)*: Destination file path for the encrypted binary payload.
- **`$skip_list`** *(array)*: Optional list of files or paths to exempt.
- **`$config`** *(array)*: Associative array of encryption and licensing constraints:
  - `"key"` *(string, optional)*: Master passkey for secondary symmetric encryption.
  - `"pass"` *(string, optional)*: Additional passphrase salt.
  - `"license"` *(string, optional)*: Customer license string (e.g. `LIC-2026-XYZ`).
  - `"device"` *(string, optional)*: The hardware ID returned by `phob_deviceID()` to lock execution to.
  - `"expiry"` *(string, optional)*: Expiration date in strict format `DD/MM/YYYY` (e.g. `31/12/2026`).

- **Returns**: `true` on success, or throws an `E_ERROR` / returns `false` on failure.

---

### 3. `phob_use()`
Securely decrypts and executes the protected file directly in PHP runtime memory.

```php
bool phob_use(string $encrypted_file_path)
```
- **`$encrypted_file_path`** *(string)*: Path to the encrypted file generated via `phob_build()`.
- **Returns**: `true` on successful execution, or halts execution with an `E_ERROR` if verification fails (expired date, wrong hardware, or corrupted payload).

---

## 🛠️ Complete Step-by-Step Example

### Step 1: Get Target Hardware ID
Provide your client with a simple one-line script to obtain their server's Device ID:

```php
<?php
// get_id.php
if (extension_loaded('phob')) {
    echo "Client Device ID: " . phob_deviceID() . PHP_EOL;
} else {
    echo "PHOB extension not loaded." . PHP_EOL;
}
```
*Client Output:*
```text
Client Device ID: VRJYS7F6RHGER4
```

---

### Step 2: Build Encrypted Package
On your build/deployment machine, encrypt your raw script:

**Raw Code (`app_core.php`):**
```php
<?php
class SecurityService {
    public function authorize() {
        return "Access Granted: Proprietary logic active.";
    }
}
$service = new SecurityService();
echo $service->authorize() . PHP_EOL;
```

**Build Script (`builder.php`):**
```php
<?php
$config = [
    "key"     => "EnterpriseMasterSecret",
    "pass"    => "AppSalt9988",
    "license" => "PRO-LICENSE-8849-B2C",
    "device"  => "VRJYS7F6RHGER4",     // Lock to client's Device ID
    "expiry"  => "31/12/2026"          // Expire at midnight on Dec 31, 2026
];

$success = phob_build('app_core.php', 'app_core.phob', [], $config);

if ($success) {
    echo "Build successful: app_core.phob generated." . PHP_EOL;
} else {
    echo "Build failed." . PHP_EOL;
}
```

---

### Step 3: Run Protected Code
On the client machine (with the PHOB extension installed):

**Entry Point (`index.php`):**
```php
<?php
// Directly load and run protected file in memory
phob_use('app_core.phob');

// Protected classes and variables are now instantiated and available
```

---

## 🔒 Licensing Rules & Enforcement Matrix

When `phob_use()` is called, the extension enforces a non-bypassable sequence of checks in kernel/native memory:

```
[Start Execution]
       │
       ▼
[Anti-Debugger Check] ──(Debugger Active)──► [Immediate Process Exit]
       │
       ▼
[Anti-Core-Dump Check (Linux)]
       │
       ▼
[Read & Parse Dynamic Header Stamp]
       │
       ▼
[Puzzle Removal & AES-256-GCM Decrypt] ──(Tampered / Invalid Key)──► [Halt with E_ERROR]
       │
       ▼
[Expiry Date Verification] ──(Now > Expiry)──► [Halt: "Service is expired"]
       │
       ▼
[Hardware Device ID Match] ──(Device != Machine)──► [Halt: "Service not authenticated"]
       │
       ▼
[Zend Compile to Opcode Memory]
       │
       ▼
[Zero-Memory Shredding of Plaintext]
       │
       ▼
[Execute in Runtime VM]
```

1. **Expiry Date Validation**:
   - Compares the server's current UTC Unix timestamp against 23:59:59 of the specified `DD/MM/YYYY`.
   - If expired: Execution aborts with `Fatal error: phob_use(): Service is expired`.

2. **Device Hardware Validation**:
   - Compares the embedded hash with the real-time generated hardware fingerprint.
   - If executed on any other machine: Execution aborts with `Fatal error: phob_use(): Service not authenticated for this device`.

---

## ❓ Troubleshooting & FAQ

#### Q1: "Call to undefined function phob_build()" or "phob_use()"
- Ensure `extension=phob.so` (Linux/macOS) or `extension=phob.dll` (Windows) is added to the **correct** `php.ini` file.
- Check active loaded configuration file with `php --ini`.

#### Q2: Can the decrypted PHP code be dumped to disk?
- **No.** PHOB does not use temporary files (`/tmp`, `sys_get_temp_dir`). All decryption occurs strictly in dynamically allocated C heap memory, compiled directly to Zend memory, and immediately wiped with `memset(0)`.

#### Q3: Does PHOB work with OPcache enabled?
- **Yes.** PHOB is fully compatible with Zend OPcache. Protected files executed through `phob_use()` execute directly via Zend opcodes.

#### Q4: How does Windows hardware detection work without `wmic`?
- Windows 11 removed `wmic.exe`. PHOB accesses the Windows Kernel Cryptographic Registry (`HKLM\SOFTWARE\Microsoft\Cryptography\MachineGuid`) directly through native Win32 `Advapi32` APIs without spawning shell sub-processes.

---

## 🛡️ Security Best Practices

1. **Keep Binaries Stripped**: Distribute pre-compiled binaries (`phob.so`, `phob.dll`) stripped of all debug symbols.
2. **Combine Device Locking with Expiration Dates**: For maximum revenue protection, bind licenses to both a client device ID and a specific support contract expiration date.
3. **Restrict Server Access**: Ensure permissions on production web servers restrict read access to configuration files and PHP logs.
4. **Never Commit Raw Master Keys**: Do not commit build scripts containing sensitive production keys to public git repositories.

---

## 🔗 Links & Resources
- **GitHub Repository**: [https://github.com/mystack-framework/phob](https://github.com/mystack-framework/phob)
- **Linux Extension**: [phob.so](https://github.com/mystack-framework/phob/blob/main/phob.so)
- **Windows Extension**: [phob.dll](https://github.com/mystack-framework/phob/blob/main/phob.dll)
