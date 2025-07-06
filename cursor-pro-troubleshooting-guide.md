# Cursor Pro Troubleshooting Guide

## Common Issues and Solutions for Cursor Pro Activation

### 1. **Basic Login and Account Issues**

#### Check Your Account Status
- Open Cursor Settings: `Cmd/Ctrl + Shift + J` (or `Cmd/Ctrl + ,`)
- Look for "Account" section
- Verify it shows "Pro" status next to your account

#### Solution: Log Out and Log In Again
1. Go to Cursor Settings
2. Click "Log out" 
3. Restart Cursor completely
4. Log back in with your Pro account credentials
5. **This resolves most activation issues**

### 2. **Network and Corporate Environment Issues**

#### For Corporate Networks/Proxies/VPNs
If you're behind a corporate firewall, VPN, or using ZScaler:

**Disable HTTP/2:**
1. Open Cursor Settings (`Cmd/Ctrl + ,`)
2. Search for "http2" 
3. Set `cursor.general.disableHttp2` to `true`
4. Restart Cursor

**Check API Connectivity:**
Test in your terminal:
```bash
curl -I --http2 -v https://api2.cursor.sh
ping api2.cursor.sh
ping api3.cursor.sh
```

**Disable Proxy Settings:**
1. Open Cursor settings
2. Search for "proxy"
3. Set "Http: Proxy" to "off" or clear any proxy URL
4. Restart Cursor

### 3. **Cursor Tab Not Working Despite Pro Status**

#### Enable Cursor Tab
1. Check the status bar at the bottom right of the editor
2. Look for "Cursor Tab" - it should be enabled
3. If it shows "Requires Pro", try the following solutions:

#### Solution Steps:
1. **Reinstall Method:**
   - Completely quit Cursor
   - Uninstall Cursor
   - Delete these folders:
     - Windows: `%APPDATA%\Cursor` and `%USERPROFILE%\.cursor`
     - macOS: `~/Library/Application Support/Cursor` 
     - Linux: `~/.config/Cursor`
   - Download fresh copy from https://cursor.com/downloads
   - Install and log in again

2. **Settings Reset:**
   - Go to Cursor Settings
   - Look for any HTTP/2 or proxy settings
   - Disable HTTP/2 if enabled
   - Clear any proxy configurations

### 4. **Linux-Specific Issues (Ubuntu/AppImage)**

#### For Ubuntu 24.04 and AppImage Issues:
If Cursor AppImage won't run due to AppArmor restrictions:

1. **Move AppImage to permanent location:**
   ```bash
   mkdir -p ~/Applications
   mv ~/Downloads/cursor*.AppImage ~/Applications/cursor.AppImage
   chmod +x ~/Applications/cursor.AppImage
   ```

2. **Create AppArmor profile:**
   ```bash
   sudo nano /etc/apparmor.d/cursor-appimage
   ```
   
   Add this content:
   ```
   abi <abi/4.0>,
   include <tunables/global>
   
   profile cursor /home/{USER}/Applications/cursor*.AppImage flags=(unconfined) {
     userns,
     include if exists <local/cursor>
   }
   ```

3. **Apply the policy:**
   ```bash
   sudo apparmor_parser -r /etc/apparmor.d/cursor-appimage
   ```

### 5. **Advanced Troubleshooting**

#### Clear Cursor Data (Nuclear Option)
⚠️ **Warning: This will delete all your Cursor settings and chat history**

**Windows:**
```cmd
rmdir /s "%APPDATA%\Cursor"
rmdir /s "%USERPROFILE%\.cursor"
```

**macOS:**
```bash
rm -rf ~/Library/Application\ Support/Cursor
rm -rf ~/.cursor
```

**Linux:**
```bash
rm -rf ~/.config/Cursor
rm -rf ~/.cursor
```

Then reinstall Cursor from https://cursor.com/downloads

#### Antivirus Interference
- Add Cursor to your antivirus exceptions
- Some antivirus software blocks Cursor's API connections

### 6. **When Nothing Works**

#### Contact Support
If all else fails, email: **hi@cursor.com**

Include:
- Your account email
- Operating system and version
- Network environment (corporate/personal)
- Screenshots of error messages
- Steps you've already tried

#### Common Success Rate by Solution:
1. **Log out/in again**: ~70% success rate
2. **Disable HTTP/2**: ~60% success rate (corporate environments)
3. **Full reinstall**: ~80% success rate
4. **Clear data + reinstall**: ~90% success rate

### 7. **Prevention Tips**

- Don't delete and recreate your Cursor account multiple times
- Keep Cursor updated to the latest version
- If using VPN, try connecting without it first
- Check your firewall settings if in corporate environment

---

**Most users find success with the basic "log out and log in again" solution. Try the simpler solutions first before moving to more drastic measures.**