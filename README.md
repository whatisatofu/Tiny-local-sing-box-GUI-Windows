# Sing-Box GUI Interface and Configuration Controller (`sb_gui_ini`)

A lightweight Windows HTML Application (HTA) interface and configuration controller for `sing-box`. It provides a compact desktop control panel to manage configuration files, validate syntax, monitor service status, and control execution with administrative privileges.

---

## 🌟 Key Features

* **Compact Desktop GUI**: Formatted as a 400x600 single-instance HTML Application window for minimal memory footprint.
* **Configuration Target Persistence**: Stores active configuration file paths in `gui_config.ini` using UTF-8 encoding and offers an automatic setup prompt when no configuration is loaded.
* **Pre-Flight Validation (`sing-box check`)**: Automatically executes a syntax check on the selected JSON configuration before launching. Diagnostic logs are output to `check_log.txt`, and startup is aborted if errors are detected.
* **Elevated Process Control**: Seamlessly executes `sing-box run` and `taskkill` commands with elevated privileges (`runas`) via Windows Shell.
* **Real-time Status Monitoring**: Continuously checks process status (`sing-box.exe`) using Windows Management Instrumentation (WMI / `Win32_Process`) and reflects real-time status indicators (🟢 Running / 🔴 Stopped).
* **Interactive File Selection**: Integrated JSON file picker to browse and switch configuration targets on the fly.

---

## 📁 File Structure

| File | Description |
| :--- | :--- |
| `sb_gui_ini.hta` | Main HTML Application script containing the user interface and control logic. |
| `gui_config.ini` | Automatically generated settings file storing the target JSON configuration path (`target=...`). |
| `check_log.txt` | Temporary log file created during pre-flight configuration validation checks. |

---

## ⚙️ Prerequisites & System Requirements

* **Operating System**: Microsoft Windows (supports HTA and ActiveX/WMI components).
* **Core Binary**: `sing-box.exe` must be installed and available in the system execution PATH or the working directory.
* ```powershell
  # Run this in powershell to install sing-box easily, although you should already have one, I suppose
  winget install sing-box
  ```
* **Permissions**: Administrative privileges are required to run network services and manage background processes.

---

## 🔒 Permissions & Security Notice

* **Standard User GUI Execution**: The HTA control window launches as a standard, non-administrative application.
* **On-Demand Privileges (`runas`)**: Starting `sing-box` (specifically when using TUN mode or modifying system routing tables) requires elevated rights. Windows UAC will request Administrator approval **only when you click "▶ Start VPN" or "⏹ Stop VPN"**.

---
## 🚀 Getting Started

1. **Launch Application**: Double-click the `.hta` script file to open the control window.
2. **Select Configuration**:
   * On first launch, the app will prompt you to select your `sing-box` `.json` configuration file.
   * Path settings are saved directly to `gui_config.ini`.
3. **Start Proxy Service**:
   * Click the **Start** button.
   * The controller runs `sing-box check -c <path>` to verify syntax. If syntax errors exist, an alert displays the detailed logs from `check_log.txt`.
   * Upon successful pre-check, elevated execution (`runas`) launches `sing-box run -c <path>`.
4. **Stop Proxy Service**:
   * Click the **Stop** button to terminate `sing-box.exe` via elevated `taskkill`.
