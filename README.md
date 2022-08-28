# Magic Carpet 2 HD Clean

Reorganize and clean code base of active fork at https://github.com/thobbsinteractive/magic-carpet-2-hd

## Changelog

## Dependencies
  - SDL2
  - SDL2-Mixer-X

## Building

  ```c
  cmake --install-prefix=/usr .
  make
  make install
  ```

  ```
  5. NOTE: The game will search in the following locations (and in this particular order) for the game assets. For the flatpak only the first two locations can be used.
     1. `$XDG_DATA_HOME/remc2/`
     2. `$HOME/.local/share/remc2`
     3. next to the `remc2` binary
  6. Run the `remc2` executable in install directory
  ```bash
  cd /Documents/repos/magic-carpet-2-hd/build/${BUILDTYPE}/inst/bin
  ./remc2
  ```

#### Configuring `remc2`

Some settings can be configured via the file `config.ini`. An example for this file can be found in the root directory of the `remc2` repository.
The game will search for this file in the following locations and this particular order. For the flatpak only the first two locations can be used.
1. `$XDG_CONFIG_HOME/remc2`
2. `$HOME/.config/remc2`
3. next to the `remc2` binary


# ROADMAP:

## MILLSTONE 1
- [x] Get solution runnable from Visual Studio 2019 build, with minimum of setup. Cut down on unnecessary extra files and libraries and use nuget instead.
- [x] Refactor reverse engineered code into seperate classes where possible.

## MILLSTONE 2
- [x] Add resolution support
- [ ] Implement Open GL render
- [ ] Implement a (platform independent) Launch menu to adjust settings in config.ini before launch

## MILLSTONE 3
- [ ] Improve sounds and music using updated original scores and directional sounds in game
- [ ] Implement a wix sharp .msi installation for new .exe to make patching the and running existing game simple and something similar for the Linux versions

## MILLSTONE 4
- [ ] Get basic LAN/IPv4 multiplayer working again (Tom is currently making great progress on this!)

## MILLSTONE 5
- [ ] Get Magic Carpet 1 working using this engine. Ideally with original music and graphics.

## LONG TERM GOALS
- Add VR support back into the game (yes it was originally supported! This game was waaay ahead of its time)<br />
- Implement online multiplayer match making

## If you know a bit about game development or want to help out, branch away or email me here: thobbsinteractive@gmail.com or find us on Discord here: https://discord.gg/mFMRUVeu

## Development Guide ##
- The Project is compiled as C++17.
- If you re-name a method include the id from the original method name as this makes it easier to track changes from the generated code.
e.g. `void sub_19CA0_sound_proc5(unsigned __int8 a1)` was renamed to `void ChangeSoundLevel_19CA0(uint8_t option)`
- Please follow the general style of the refactored code. Upper Camel Case (Pascal Case) for Class/Method names. Camel Case for variables. 'm_' for class members. `GameRenderHD.cpp` is a good example of the style.
- Where possible (if writting new code) please use the fixed width types. https://en.cppreference.com/w/cpp/types/integer
- Be careful with making logic changes to the code and Test, Test, Test! I recommend playing the first level all the way though. Then the first Cave level (4) and I also recomend Level 5 as you have a nice mix of AI to kill and a cutscene at level completion.
