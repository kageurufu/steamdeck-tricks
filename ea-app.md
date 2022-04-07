# EA App Beta in Proton

## Getting required files

```sh
# Grab wine-mono and wine-gecko
mkdir -p ~/.cache/wine
curl -o ~/.cache/wine/wine-mono-7.2.0-x86.msi      https://dl.winehq.org/wine/wine-mono/7.2.0/wine-mono-7.2.0-x86.msi
curl -o ~/.cache/wine/wine-gecko-2.47.2-x86.msi    https://dl.winehq.org/wine/wine-gecko/2.47.2/wine-gecko-2.47.2-x86.msi
curl -o ~/.cache/wine/wine-gecko-2.47.2-x86_64.msi https://dl.winehq.org/wine/wine-gecko/2.47.2/wine-gecko-2.47.2-x86_64.msi

# We need winetricks available. You can use protontricks instead if you want
curl -o ~/Downloads/winetricks                     https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks
chmod +x ~/Downloads/winetricks

# Grab the EA App Installer
curl -o ~/Downloads/EAappInstaller.exe             https://origin-a.akamaihd.net/EA-Desktop-Client-Download/installer-releases/EAappInstaller.exe

# Get protontricks
flatpak install com.github.Matoking.protontricks
```

## Preparing your EA App prefix in Steam

1. Add EAappInstaller.exe as a non-Steam game
  1. "ADD A GAME"
  2. "BROWSE..."
  3. File type: "All files"
  4. Browse to, and "OPEN" /home/deck/Downloads/EAappInstaller.exe
  5. Make sure EAappInstaller.exe is checked, then click "ADD SELECTED PROGRAMS"
  6. In your Steam library, edit the EAappInstaller.exe properties:
    * Change the shortcut name to "EA App"
    * Switch to the Compatibility tab
        * Check Force the use of a specific Steam Play compatibility tool
        * Select a Proton version, I've only tested Proton Experimental right now
2. Run "EA App" from Steam once to create your prefix, close out the installer once it opens

## Locate your prefix

Open ~/.local/share/Steam/steamapps/compatdata/, and find the newest folder there. You can also use Protontricks to find the ID directly. 
Mine was 3073228955, but the number doesn't really matter here

## Install some dependencies

You need to replace my 3073228955 with your ID here!

```sh
export STEAM_APP_ID=3073228955

# Setup
flatpak run com.github.Matoking.protontricks $STEAM_APP_ID corefonts d3dx9 d3dcompiler_43 d3dcompiler_47
flatpak run com.github.Matoking.protontricks -c 'wine msiexec /i "~/.cache/wine/wine-mono-7.2.0-x86.msi"' $STEAM_APP_ID
flatpak run com.github.Matoking.protontricks -c 'wine msiexec /i "~/.cache/wine/wine-gecko-2.47.2-x86.msi"' $STEAM_APP_ID
flatpak run com.github.Matoking.protontricks -c 'wine msiexec /i "~/.cache/wine/wine-gecko-2.47.2-x86_64.msi"' $STEAM_APP_ID
```

## Install the EA app from Steam

Run EAappInstaller.exe from Steam, it will take a few minutes to install, then will auto launch the app.

## Fix the Steam app details

Go back to steam, and edit the properties again.
You can download grid images from https://www.steamgriddb.com/game/5306742, or use [steamgrid](https://github.com/boppreh/steamgrid) (you should be using steamgrid anyway to auto-populate grid images for all your non-steam games and apps)

* Set "TARGET" to "/home/deck/.steam/steam/steamapps/compatdata/3073228955/pfx/drive_c/Program Files/Electronic Arts/EA Desktop/EA Desktop/EADesktop.exe"
  * This should auto-update "START IN", if not you need to set it to "/home/deck/.steam/steam/steamapps/compatdata/3073228955/pfx/drive_c/Program Files/Electronic Arts/EA Desktop/EA Desktop/"

## Installed other EA Games, and adding natively to Steam 

Launch the EA App, login, and install the games you want. I highlight Mass Effect Legendary Edition next, but it will probably work for most other EA games too.

After the install is finally complete, fully exit EA Desktop.

### Mass Effect Legendary Edition

* Add a non-steam game
  * `/home/deck/.steam/steam/steamapps/compatdata/3073228955/pfx/drive_c/Program Files/EA Games/Mass Effect Legendary Edition/Game/ME1/Binaries/Win64/MassEffect1.exe`
  * `/home/deck/.steam/steam/steamapps/compatdata/3073228955/pfx/drive_c/Program Files/EA Games/Mass Effect Legendary Edition/Game/ME2/Binaries/Win64/MassEffect2.exe`
  * `/home/deck/.steam/steam/steamapps/compatdata/3073228955/pfx/drive_c/Program Files/EA Games/Mass Effect Legendary Edition/Game/ME3/Binaries/Win64/MassEffect3.exe`
* Edit the properties for the new games
  * Rename to "Mass Effect (2007)", "Mass Effect 2", and "Mass Effect 3 (2012)" to support `steamgrid`
  * Set launch options to `STEAM_COMPAT_DATA_PATH="/home/deck/.steam/steam/steamapps/compatdata/3073228955/" %command%`
    * This will force Steam to use the same proton configuration for Mass Effect as the EA App
  * Switch to the Compatibility tab
      * Check Force the use of a specific Steam Play compatibility tool
      * Select a Proton version, I've only tested Proton Experimental right now
If you want to use the Mass Effect Launcher for whatever reason

* Add a non-steam game
  * Add `/home/deck/.steam/steam/steamapps/compatdata/3073228955/pfx/drive_c/Program Files/EA Games/Mass Effect Legendary Edition/Game/ME1/Binaries/Win64/MassEffect1.exe`
* Edit the properties for the new game
  * Rename to "Mass Effect (2007)" or "Mass Effect Legendary Edition" depending on what images you want loaded
  * Set launch options to `STEAM_COMPAT_DATA_PATH="/home/deck/.steam/steam/steamapps/compatdata/3073228955/" %command%`
    * This will force Steam to use the same proton configuration for Mass Effect as the EA App