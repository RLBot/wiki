---
search:
  boost: 0.5
---

## Automated Solution

RLBot has built in support for custom maps. It allows you to select a folder that has custom map (\`\*.upk) files anywhere in the directory. Once added, you can select any of the custom maps in the directory and play with bots on that map.

### Steps

1. Run RLBot. Add -> Load Folder and select a directory with custom maps
2. In Match Settings, click on the "Map" dropdown and you should see all the `*.upk` files listed there.
3. Select and press "Start Match"

## Manual Solution

1. Find the workshop map you want to use. It will be located at
   `\Steam\steamapps\workshop\content\252950` Find the map you want inside one of those folders.
   The map has a .udk extension.

2. Change the map name to Labs_Underpass_P.upk. Be sure to change the extension as well.

3. Copy the map to C:\\Program Files (x86)\\Steam\\steamapps\\common\\rocketleague\\TAGame\\CookedPCConsole. It will replace the Underpass map.

4. On RLBotGui select Underpass to play the custom map.
