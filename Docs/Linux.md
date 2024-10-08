# YgoMaster for Linux

- Install wine
- Run `wine regedit` and do this to fix input on window focus change https://askubuntu.com/questions/299286/how-to-recover-focus-after-losing-it-while-using-wine/1202472#1202472
- Download the latest YgoMaster release from https://github.com/pixeltris/YgoMaster/releases
- Copy the `YgoMaster` folder (the folder, not the contents of the folder) into the game folder
- [Download this zip](https://github.com/user-attachments/files/16606063/YgoMaster-mono.zip) and extract / merge it with the YgoMaster folder from the previous step
- Run `wine MonoRun.exe YgoMasterClient.exe`

## Folder structure

Your folder structure should look like this:

```md
Master Duel
├── YgoMaster
│   ├── Data/
│   ├── mono/
│   ├── MonoRun.exe
│   ├── YgoMasterClient.exe
│   ├── YgoMaster.exe
│   ├── YgoMasterLoader.dll
├── ...
```

Make sure you copied `mono` / `MonoRun.exe` from the zip into the correct place as noted into the above folder structure.

## Missing fonts

You may have missing fonts under wine. If the game crashes when prompted to load files / folders in the custom sub menus (i.e. loading decks from files) try the following:

- Try adding the `Tahoma` font using `winetricks` (this worked for me)
  - Run `winetricks`
  - Select the default wineprefix
  - Install a font
  - Check `tahoma` and press OK
- Try installing `sudo apt-get install ttf-mscorefonts-installer`
- Diagnose the font issue yourself
  - Run `WINEDEBUG=+font wine MonoRun.exe YgoMasterClient.exe`
  - Go to the deck editor, open the sub menu in the top right and click to load a deck from a file
  - The game will crash
  - Check the console output for the last font that tried to load
  - Install the mentioned font with winetricks

## Diagnosing issues

Create `mono_log.txt` in the Master Duel folder and inside the `YgoMaster` folder. This will log all mono messages in these files which you can look through to diagnose problems with mono.

## Where the `mono` files come from

The files inside `mono` were copied from the latest mono stable release https://www.mono-project.com/download/stable/#download-win

- `%ProgramFiles%/mono/lib/mono/4.5/` folder was stripped down and copied to the output `mono/lib`. Additionally `mscorlib.dll` was copied from the `4.5` folder into `mono/lib/mono/4.5/mscorlib.dll` as that is where mono wants it
- `%ProgramFiles%/mono/etc/` was copied as-is to the output `mono/etc`
- `%ProgramFiles%/mono/bin/*.dll` was copied as-is to the output `mono/bin`

## Bugs

- `ClientSettings.json` setting `ShowConsole` does not work