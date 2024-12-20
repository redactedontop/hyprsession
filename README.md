# Hyprsession
## Overview
Implements session persistance for Hyprland. While the program is running it periodicly saves the command, workspace and other properties of running clients found by `hyprctl clients`. These are then saved to a file formatted as a Hyprland config file which can then be sourced so that the session is restored when Hyprland is restarted.

## Installation
As root run the command 
```
cargo install --locked --root /usr/local --git https://github.com/tiecia/hyprsession
``` 
Or install as a user by replacing `/usr/local` with your home directory. Then add the following line to your Hyprland config file (Usually at ~/.config/hypr/hyprland.conf)
```
exec_once = hyprsession
```
If you want to save a session that is already running then run
```
hyprsession --mode save-only &
```
or
```
hyprsession --mode save-and-exit
```

#### NixOS
Add the input to your `flake.nix`
```
hyprsession.url = "github:tiecia/hyprsession"
```
To automatically run it with home-manager add the following to your `home.nix` hyprland configuration
```
exec-once = "${inputs.hyprsession.packages.${pkgs.system}.hyprsession}/bin/hyprsession"
```

## Options
Various options can be used to modify the behavior of Hyprsession.

### --mode <mode>
Sets the mode the program runs in 
* Default - Loads the session at startup the saves the current session at regular intervals.
* SaveOnly - As above but skips loading the session
* LoadAndExit - Load the saved session then immediatly exit
* SaveAndExit - Save the current session then exit

### --save-interval n
This sets the interval in seconds between session saves. The default is 60 seconds.

### --session-path
This allows the user to save the session config in an alternative directory, by default its ~/.local/share/hyprsession. 

## TODO
* Create and use a rules file for alternative handling of applications (i.e. do not reload, ignore parameters, additional parameters etc).
* Handle application that create windows in forked processes by creating temporary window rules.

## Change log
### 0.1.1
* Changed --session-path option to point at base directory of session file
### 0.1.2
* Fixed bug which would crash program if no session file existed
### 0.1.3
* Fix fatal bug on Hyprland 0.4 that crashed the program while saving the session  
### 0.1.4
* Changed fullscreen dispatcher to use fullscreenmode and fixed FullscreenMode type issue (#2)
* Updated to latest alpha version of hyprland crate. Fixes panic on fetching clients (#1)
### 0.1.5
* Added accurate README instructions for installation
