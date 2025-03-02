# Keyboard Manager for Linux

## 🎛️ Automatic Internal/External Keyboard Switching

Keyboard Manager is a bash script solution for Linux that automatically the internal laptop keyboard when an external keyboard is connected, and re-enables it when the external keyboard is disconnected.

![Platform](https://img.shields.io/badge/Platform-Linux-blue.svg)

## 🚀 Features

- **Custom keyboard selection**: Select which keyboard is your external keyboard from a list of connected devices
- **Automatic switching**: Internal keyboard is disabled when external keyboard connects, re-enabled when it disconnects
- **Management commands**: Simple commands to check status or remove configuration

## ⚙️ Installation/Usage

1. Clone this repository or download the script:

   ```bash
   git clone https://github.com/yourusername/keyboard-manager.git
   cd keyboard-manager
   ```

2. Make the script executable:

   ```bash
   chmod +x keyboard-setup.sh
   ```

3. Run the setup script with sudo:

   ```bash
   sudo ./keyboard-setup.sh
   ```

4. Follow the prompts to select your external keyboard.

## 🔧 Usage

After installation, you can manage your keyboard configuration with the following commands:

```bash
# Check current keyboard configuration
sudo keyboard-manager status

# Remove keyboard management (restore normal behavior)
sudo keyboard-manager remove

# View help and available commands
sudo keyboard-manager help
```

## 🔄 How It Works

The script leverages Linux's udev system to manage device binding/unbinding at the kernel level rather than the graphical environment

1. Creates udev rules in `/etc/udev/rules.d/85-keyboard-management.rules`
2. Rules target specifically the selected external keyboard via its exact name in the ATTRS{name} field
3. When connected (ACTION=="add"), it executes commands to:
   - Change the serio0 bind_mode to "manual" (disables auto-binding)
   - Explicitly unbinds the internal keyboard by writing its ID to the atkbd unbind sysfs node
4. When disconnected (ACTION=="remove"), it executes commands to:
   - Change the serio0 bind_mode back to "auto" (enables auto-binding)
   - Explicitly binds the internal keyboard by writing its ID to the atkbd bind sysfs node
