If RLBotGUI can't open, won't start a match, or acts in other strange ways it may be related to your antivirus software. Do not disable your antivirus.

### If you suspect your antivirus has broken RLBot:

1. Press `Windows + R`, and type in `%localappdata%\RLBotGUIX` and then press Enter
2. Delete the folders `Python37` and `venv`
3. Redownload RLBotGUI if necessary
4. Create an exception for RLBotGUI in your antivirus software. The executable is located at `C:\Program Files\RLBot`
5. Launch RLBotGUI

You might also be having a problem with your firewall. Windows Defender Firewall has been known to wrongly block RLBotGUI.exe in the past. To fix this problem:

01. Go to Control Panel
02. System and Security
03. Under Windows Defender Firewall, click "Allow an app through Windows Firewall"
04. Click "Change settings" (In the top-left-ish)
05. Click "Allow another app" (In the bottom-left-ish)
06. Navigate to RLBotGUI and select it.
07. Click "Network types..."
08. Make sure that both Private and Public networks are checked
09. Click "OK"
10. Click "Add"
11. Click "OK"

You're done!
