# PHOB — PHP High-Performance Obfuscator & Licensing Guard

[![PHP Version](https://img.shields.io/badge/PHP-8.1%20%7C%208.2%20%7C%208.3%20%7C%208.4%20%7C%208.5-blue.svg)](https://www.php.net)
[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20(x64)-brightgreen.svg)](#-system-requirements)
[![GitHub Repository](https://img.shields.io/badge/GitHub-mystack--framework%2Fphob-blue?logo=github)](https://github.com/mystack-framework/phob)

**PHOB** is an enterprise-grade high-performance PHP C-extension providing source code encryption, hardware-bound licensing, and direct in-memory execution for PHP 8.1, 8.2, 8.3, 8.4, and 8.5 on both Windows and Linux platforms.

---

## 📑 Table of Contents
1. [Core Capabilities](#-core-capabilities)
2. [Supported Platforms & PHP Versions](#-supported-platforms--php-versions)
3. [Binary Packages & Direct Download Links](#-binary-packages--direct-download-links)
4. [Installation & Setup Guide](#-installation--setup-guide)
   - [Windows Installation (WampServer, XAMPP, IIS, CLI)](#1-windows-installation)
   - [Linux Installation (Ubuntu, Debian, RHEL, aaPanel, Docker)](#2-linux-installation)
5. [API Reference & Usage](#-api-reference--usage)
   - [`phob_deviceID()`](#1-phob_deviceid)
   - [`phob_build()`](#2-phob_build)
   - [`phob_use()`](#3-phob_use)
6. [Complete Step-by-Step Workflow](#-complete-step-by-step-workflow)
   - [Step 1: Obtain Target Hardware ID](#step-1-obtain-target-hardware-id)
   - [Step 2: Build Encrypted Package (`.phob`)](#step-2-build-encrypted-package)
   - [Step 3: Execute Protected Code in Application](#step-3-execute-protected-code-in-application)
7. [Licensing Rules, Verification & Errors](#-licensing-rules-verification--errors)
8. [Troubleshooting & FAQ](#-troubleshooting--faq)

---

## 🚀 Core Capabilities

- **Pure In-Memory Execution**: Protected files are decrypted directly into volatile memory and compiled into Zend bytecode. Plain source code is never written to temporary files or persistent disk storage.
- **Immediate Memory Zeroing**: Memory buffers containing plaintext are automatically wiped (`memset`) immediately after Zend opcode compilation.
- **Hardware-Level Device Locking**: Cryptographically binds protected packages to the target machine's unique hardware signature.
- **Granular Expiration Dates**: Enforces license expiration down to midnight of the specified `DD/MM/YYYY`.
- **Zend Hook Protection**: Employs compiler guards to prevent untrusted extensions from hooking or dumping script bytecode.
- **Stripped Production Binaries**: All shared libraries (`.so` / `.dll`) are stripped of debugging symbols for maximum production security.

---

## 💻 Supported Platforms & PHP Versions

| Operating System | Architecture | Supported PHP Versions | Thread Safety |
| :--- | :--- | :--- | :--- |
| **Windows** (10, 11, Server) | x64 | PHP 8.1, 8.2, 8.3, 8.4, 8.5 | **ZTS** (Thread Safe - WampServer / Apache) |
| **Linux** (Ubuntu, Debian, RHEL, CentOS, aaPanel) | x86_64 | PHP 8.1, 8.2, 8.3, 8.4, 8.5 | **NTS** (Non-Thread Safe - FPM / CLI / Nginx) |

---

## ⬇️ Binary Packages & Direct Download Links

The pre-compiled production extensions are organized by operating system and PHP version in the official GitHub repository:

### 🪟 Windows Packages (`.dll`)
Repository directory: [https://github.com/mystack-framework/phob/tree/main/windows](https://github.com/mystack-framework/phob/tree/main/windows)

| PHP Version | Extension Binary File | Direct Download Link |
| :---: | :---: | :--- |
| **PHP 8.1** | `phob.dll` | [https://github.com/mystack-framework/phob/raw/main/windows/php8.1/phob.dll](https://github.com/mystack-framework/phob/raw/main/windows/php8.1/phob.dll) |
| **PHP 8.2** | `phob.dll` | [https://github.com/mystack-framework/phob/raw/main/windows/php8.2/phob.dll](https://github.com/mystack-framework/phob/raw/main/windows/php8.2/phob.dll) |
| **PHP 8.3** | `phob.dll` | [https://github.com/mystack-framework/phob/raw/main/windows/php8.3/phob.dll](https://github.com/mystack-framework/phob/raw/main/windows/php8.3/phob.dll) |
| **PHP 8.4** | `phob.dll` | [https://github.com/mystack-framework/phob/raw/main/windows/php8.4/phob.dll](https://github.com/mystack-framework/phob/raw/main/windows/php8.4/phob.dll) |
| **PHP 8.5** | `phob.dll` | [https://github.com/mystack-framework/phob/raw/main/windows/php8.5/phob.dll](https://github.com/mystack-framework/phob/raw/main/windows/php8.5/phob.dll) |

### 🐧 Linux Packages (`.so`)
Repository directory: [https://github.com/mystack-framework/phob/tree/main/linux](https://github.com/mystack-framework/phob/tree/main/linux)

| PHP Version | Extension Binary File | Direct Download Link |
| :---: | :---: | :--- |
| **PHP 8.1** | `phob.so` | [https://github.com/mystack-framework/phob/raw/main/linux/php8.1/phob.so](https://github.com/mystack-framework/phob/raw/main/linux/php8.1/phob.so) |
| **PHP 8.2** | `phob.so` | [https://github.com/mystack-framework/phob/raw/main/linux/php8.2/phob.so](https://github.com/mystack-framework/phob/raw/main/linux/php8.2/phob.so) |
| **PHP 8.3** | `phob.so` | [https://github.com/mystack-framework/phob/raw/main/linux/php8.3/phob.so](https://github.com/mystack-framework/phob/raw/main/linux/php8.3/phob.so) |
| **PHP 8.4** | `phob.so` | [https://github.com/mystack-framework/phob/raw/main/linux/php8.4/phob.so](https://github.com/mystack-framework/phob/raw/main/linux/php8.4/phob.so) |
| **PHP 8.5** | `phob.so` | [https://github.com/mystack-framework/phob/raw/main/linux/php8.5/phob.so](https://github.com/mystack-framework/phob/raw/main/linux/php8.5/phob.so) |

---

## 📦 Installation & Setup Guide

### 1. Windows Installation
Supported on **WampServer**, **XAMPP**, **IIS**, and standalone **PHP CLI**.

#### Step 1: Identify Extension Directory
Run this command in PowerShell or Command Prompt:
```powershell
php -r "echo ini_get('extension_dir');"
```
*(Typical paths: `C:\wamp64\bin\php\php8.3.28\ext\` or `C:\xampp\php\ext\`)*

#### Step 2: Download & Copy `phob.dll`
Download the `phob.dll` matching your active PHP version from [Windows Packages](https://github.com/mystack-framework/phob/tree/main/windows) and place it into that `ext` directory as `php_phob.dll` (or `phob.dll`).

#### Step 3: Enable in `php.ini`
Open your active `php.ini` file (locate it with `php --ini`) and append:
```ini
extension=php_phob
```
*(Or `extension=phob.dll` if placed as `phob.dll`)*

#### Step 4: Restart Web Server & Verify
- **WampServer / XAMPP**: Restart Apache from the system tray menu.
- **IIS**: Run `iisreset` in elevated Command Prompt.
- **CLI Verification**:
  ```powershell
  php -m
  ```
  Ensure `phob` is listed among the loaded modules.

---

### 2. Linux Installation
Supported on **Ubuntu**, **Debian**, **CentOS / RHEL**, and control panels like **aaPanel** or **cPanel**.

#### Step 1: Automated Download to Extension Directory
Run the following shell commands for your target PHP version (e.g. PHP 8.3):

```bash
# 1. Detect extension directory
EXT_DIR=$(php -r "echo ini_get('extension_dir');")

# 2. Download matching binary (replace 8.3 with your version: 8.1, 8.2, 8.3, 8.4, 8.5)
sudo curl -sSL -o "${EXT_DIR}/phob.so" "https://github.com/mystack-framework/phob/raw/main/linux/php8.3/phob.so"

# 3. Set proper execution permissions
sudo chmod 755 "${EXT_DIR}/phob.so"
```

#### Step 2: Enable in PHP Configuration
- **Ubuntu / Debian Standard:**
  ```bash
  echo "extension=phob.so" | sudo tee /etc/php/8.3/mods-available/phob.ini
  sudo phpenmod phob
  ```

- **aaPanel (e.g., PHP 8.3 at `/www/server/php/83`):**
  ```bash
  echo "extension=phob.so" | sudo tee -a /www/server/php/83/etc/php.ini
  ```

- **RHEL / CentOS / General `php.ini`:**
  ```bash
  echo "extension=phob.so" | sudo tee -a $(php -r "echo php_ini_loaded_file();")
  ```

#### Step 3: Restart PHP Services
```bash
# For Ubuntu/Debian PHP-FPM:
sudo systemctl restart php8.3-fpm

# For aaPanel:
sudo /etc/init.d/php-fpm-83 restart

# For Apache:
sudo systemctl restart apache2 # or httpd
```

#### Step 4: Verify
```bash
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

## 📖 API Reference & Usage

The PHOB extension provides three primary functions:

### 1. `phob_deviceID()`
Returns the unique hardware identifier of the machine currently executing PHP.

```php
string phob_deviceID(void)
```
- **Return Value**: A 14-character alphanumeric uppercase hardware hash (e.g., `VRJYS7F6RHGER4`).
- **Purpose**: Run this function on the client's production server to determine the target Device ID before building a hardware-locked package.

---

### 2. `phob_build()`
Packages, secures, and compiles raw PHP code into a protected distribution `.phob` file.

```php
bool phob_build(
    string $input_path, 
    string $output_path, 
    array $skip_list = [], 
    array $config = []
)
```

#### Parameters:
- **`$input_path`** *(string)*: Path to the raw PHP source file to protect.
- **`$output_path`** *(string)*: Destination file path for the generated protected payload (conventionally ending in `.phob`).
- **`$skip_list`** *(array)*: Optional list of files or sub-paths to exclude.
- **`$config`** *(array)*: Associative array of security configuration and licensing parameters:

| Config Key | Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `"key"` | `string` | Master secret key for envelope packaging. | `"MyEnterpriseSecret2026"` |
| `"pass"` | `string` | Additional security salt passphrase. | `"CustomSaltVal#99"` |
| `"license"` | `string` | Customer or deployment license identifier. | `"LIC-CORP-98421"` |
| `"device"` | `string` | Lock execution to this hardware Device ID. | `"VRJYS7F6RHGER4"` |
| `"expiry"` | `string` | Expiry date in strict `DD/MM/YYYY` format. | `"31/12/2026"` |

- **Return Value**: `true` on successful build, `false` on failure.

---

### 3. `phob_use()`
Decrypts and executes the protected `.phob` package directly in PHP runtime memory.

```php
bool phob_use(string $encrypted_file_path)
```
- **`$encrypted_file_path`** *(string)*: Path to the `.phob` file generated by `phob_build()`.
- **Return Value**: `true` on successful execution. Halts execution or raises an `E_ERROR` / `E_WARNING` if verification fails.

---

## 🛠️ Complete Step-by-Step Workflow

### Step 1: Obtain Target Hardware ID
Provide the client with a one-line script to retrieve their server's Device ID:

```php
<?php
// get_device_id.php
if (extension_loaded('phob')) {
    echo "Server Device ID: " . phob_deviceID() . PHP_EOL;
} else {
    echo "Error: phob extension is not loaded." . PHP_EOL;
}
```
*Client Output:*
```text
Server Device ID: VRJYS7F6RHGER4
```

---

### Step 2: Build Encrypted Package
On your development or deployment build machine, secure your proprietary code:

**Source File (`PaymentGateway.php`):**
```php
<?php
class PaymentGateway {
    public function processTransaction($amount, $customerEmail) {
        return [
            'status'         => 'SUCCESS',
            'transaction_id' => 'TXN_' . strtoupper(bin2hex(random_bytes(8))),
            'amount'         => $amount,
            'tax'            => $amount * 0.05,
            'total_charged'  => $amount * 1.05,
            'customer'       => $customerEmail,
            'timestamp'      => date('Y-m-d H:i:s'),
            'verified'       => 1
        ];
    }
}
```

**Build Script (`build.php`):**
```php
<?php
$config = [
    "key"     => "EnterpriseMasterSecretKey2026",
    "pass"    => "AppDeploymentSalt#8841",
    "license" => "LIC-CLIENT-PRO-2026",
    "device"  => "VRJYS7F6RHGER4",     // Lock to client machine
    "expiry"  => "31/12/2026"          // Valid until Dec 31, 2026
];

$success = phob_build(
    __DIR__ . '/PaymentGateway.php',
    __DIR__ . '/PaymentGateway.phob',
    [],
    $config
);

if ($success) {
    echo "Build SUCCESS: PaymentGateway.phob generated." . PHP_EOL;
} else {
    echo "Build FAILED." . PHP_EOL;
}
```

---

### Step 3: Execute Protected Code in Application
Deploy `PaymentGateway.phob` to the client server. The client application loads and executes it seamlessly:

**Client Entry Script (`index.php`):**
```php
<?php
// 1. Verify extension
if (!extension_loaded('phob')) {
    die("Fatal: PHOB extension is required.");
}

// 2. Load protected module directly into memory
$loaded = phob_use(__DIR__ . '/PaymentGateway.phob');
if (!$loaded) {
    die("Failed to execute protected code.");
}

// 3. Classes from PaymentGateway.phob are now available in runtime
$gateway = new PaymentGateway();
$result = $gateway->processTransaction(250.00, 'customer@example.com');

print_r($result);
```

*Output:*
```text
Array
(
    [status] => SUCCESS
    [transaction_id] => TXN_564FBC818D2C8885
    [amount] => 250
    [tax] => 12.5
    [total_charged] => 262.5
    [customer] => customer@example.com
    [timestamp] => 2026-10-27 15:34:09
    [verified] => 1
)
```

---

## 🔒 Licensing Rules, Verification & Errors

When `phob_use()` is called, the extension performs real-time validation checks:

```
[phob_use() Called]
        │
        ▼
[Integrity Verification] ──(Payload Tampered / Corrupted)──► "File integrity check failed"
        │
        ▼
[License Expiry Check]   ──(Current Date > Expiry)────────► "Service is expired"
        │
        ▼
[Device ID Verification] ──(Device Hash Mismatch)────────► "Service not authenticated for this device"
        │
        ▼
[In-Memory Zend Compilation & Opcode Execution]
        │
        ▼
[Zero-Wipe Plaintext Memory Buffer]
```

### Common Errors & Explanations:

| Error Notice / Warning | Cause | Resolution |
| :--- | :--- | :--- |
| `phob_use(): Service is expired` | The server's current date has exceeded the `"expiry"` date set during `phob_build()`. | Rebuild the `.phob` file with an updated `"expiry"` date for the license renewal. |
| `phob_use(): Service not authenticated for this device` | The `.phob` file was locked to a different Device ID than the machine executing it. | Verify `phob_deviceID()` on the target machine and rebuild the package for that specific Device ID. |
| `phob_use(): File integrity check failed` | The `.phob` file was modified, truncated, or corrupted in transit. | Re-upload the `.phob` file using binary transfer mode. |
| `Call to undefined function phob_use()` | The `phob` extension is not loaded in PHP. | Verify `extension=php_phob` (Windows) or `extension=phob.so` (Linux) in `php.ini`. |
| `Failed to execute protected code` | `phob_use()` returned `false` due to failed authorization or compilation. | Check PHP error logs for specific diagnostic warnings. |

---

## ❓ Troubleshooting & FAQ

#### Q1: Where can I download pre-built binaries for my PHP version?
- Windows builds: [https://github.com/mystack-framework/phob/tree/main/windows](https://github.com/mystack-framework/phob/tree/main/windows)
- Linux builds: [https://github.com/mystack-framework/phob/tree/main/linux](https://github.com/mystack-framework/phob/tree/main/linux)

#### Q2: Is OPcache supported?
**Yes.** PHOB integrates directly with Zend OPcache. Decrypted opcodes are executed natively without performance penalty.

#### Q3: How do I remove license restrictions for trial distributions?
If you do not want to restrict by hardware or expiration date, simply omit `"device"` and `"expiry"` from the `$config` array:
```php
$config = [
    "key"  => "YourSecretKey",
    "pass" => "YourSalt"
];
```

#### Q4: How is the Windows Device ID generated without `wmic`?
Windows 11 removed `wmic.exe`. PHOB uses the native Windows Win32 Registry API directly (`Advapi32`) to query `MachineGuid` from `HKLM\SOFTWARE\Microsoft\Cryptography`, ensuring native compatibility without external processes.

---

## 🔗 Official Links & Resources
- **GitHub Repository**: [https://github.com/mystack-framework/phob](https://github.com/mystack-framework/phob)
- **Windows Extensions**: [https://github.com/mystack-framework/phob/tree/main/windows](https://github.com/mystack-framework/phob/tree/main/windows)
- **Linux Extensions**: [https://github.com/mystack-framework/phob/tree/main/linux](https://github.com/mystack-framework/phob/tree/main/linux)
