# Resources:

[linux mint video source](https://www.youtube.com/watch?v=XC0JonvTTOk)
[windows kiosk video source](https://www.youtube.com/watch?v=4ESI2HCGzHI)

---
# On the remote desktop:
### 1. Download Rustdesk [click here](https://github.com/rustdesk/rustdesk/releases/tag/1.4.0).

**1.1. Click on the EXE under Windows x86-64 (64-bit).**

### 2. Open the .exe file you just downloaded and click install.

### 3. Click on the three dots next to the ID.

![[assets/rustdesk-opened.png]]

### 4. Click on `Unlock security settings`. 

![[assets/unlock-rustdesk-security.png]]
### 5. Under the `Password` section select `Use permanent password`:

![[assets/choose-use-permanent-passowrd-rustdesk.png]]

**5.1. Set a permanent password:**
![[assets/set-rustdesk-password.png]]

### 6. Under security: choose `Enable direct IP access` and enter your desired port, default_port: `21118`.

![[assets/enable-direct-ip-access-rustdesk.png]]

### 7. Open port `21118` in the Windows Firewall:

>Note: if you change the port then use your port instead.

**7.1. Open start menu and open firewall then click on `Advanced settings`.**

![[assets/firewall.png]]

**7.2. Click on `Inbound Rules` on the left of the window.**

![[assets/inbound-rules.png]]

**7.3. Click on `New Rules` on the right of the window.**

![[assets/new-rule.png]]

**7.4. Select `Port` in the middle then click `Next`.**

![[assets/select-port.png]]

**7.5. Type the Desired port then click `Next`.**

![[assets/add-port.png]]

**7.6. Click `Next`.**

![[assets/allow-all-connection.png]]

**7.7. Click `Next`.**

![[assets/network-types.png]]

**7.8. Name this rules then click `Finish`.**

![[assets/name-the-rule.png]]

**7.9. Don't forget to delete the `RustDesk Service` rule.**

![[assets/delete-rustdesk-service-rule.png]]
# On your device:

### 1. Download Rustdesk [click here](https://github.com/rustdesk/rustdesk/releases/tag/1.4.0).

**1.1. Click on the EXE under Windows x86-64 (64-bit).**

### 2. Open the .exe file you just downloaded and click install.

### 3. Enter the IP of your remote device in `Enter remote ID` box then click `connect`.

>If you change the default port: 10.0.0.20:PORT EXP:10.0.0.20:8914

![[assets/connect-rustdesk.png]]

**Enter the password that you created on the remote desktop Rustdesk app:**

>Note: not physical machine user login password.

![[assets/connect-enter-password-rustdesk.png]]

**Remote Desktop:**
![[assets/new-chrome-shortcut.png]]