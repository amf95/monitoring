
# Windows:

## A: Auto Start Chrome:
### 1. make sure chrome is installed.

### 2. create new chrome shortcut:

**2.1. Just copy `ctrl+c` and past `ctrl+v` the already existing chrome shortcut then rename it. EXP: name it `grafana-dashboard`.**

![[Services/monitoring/graphical-user-interface/monitoring-kiosk-screen-windows-passets/new-chrome-shortcut.png]]
### 3. Right click on the new chrome shortcut then click properties:

![[Services/monitoring/graphical-user-interface/monitoring-kiosk-screen-windows-passets/right-click-new-shortcut.png]]

**3.1. In the target box after `"C:\Program Files\Google\Chrome\Application\chrome.exe"`  leave a space then paste the link to your desired dashboard:**

**Normal mode:**
>EXP: `"C:\Program Files\Google\Chrome\Application\chrome.exe" http://10.0.0.20:3000/dashboards`

**Kiosk mode:** `--kiosk`
>EXP: `"C:\Program Files\Google\Chrome\Application\chrome.exe" --kiosk http://10.0.0.20:3000/dashboards`
>
>**To Escape kiosk mode hit `ctrl+alt+del` on the remote machine itself.**
>
>Hint: you cant select task manager and kill chrome or do something else.

**Full screen mode:** `--start-fullscreen`
>EXP: `"C:\Program Files\Google\Chrome\Application\chrome.exe" --start-fullscreen http://10.0.0.20:3000/dashboards`


>Note: make sure your link is short cause number of characters is limited.

![[Services/monitoring/graphical-user-interface/monitoring-kiosk-screen-windows-passets/target-box.png]]
### 4. Now open start menu, open `Run` then enter the following command: `shell:startup`.

![[Services/monitoring/graphical-user-interface/monitoring-kiosk-screen-windows-passets/search-for-run.png]]

![[Services/monitoring/graphical-user-interface/monitoring-kiosk-screen-windows-passets/run-command.png]]

**4.1. This will open the following folder: `C:\Users\admin\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup`.**

**4.2. Now move or copy the chrome shortcut you just created to this folder.**

>Note: if you want to modify the link later then open the shortcut in the `C:\Users\admin\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup` folder and do step #4.2. and #3. 

![[Services/monitoring/graphical-user-interface/monitoring-kiosk-screen-windows-passets/startup-folder.png]]
### 5. Make sure chrome is enabled as a startup program.

5.1. In the start menu search for startup:

![[Services/monitoring/graphical-user-interface/monitoring-kiosk-screen-windows-passets/startup-settings.png]]

