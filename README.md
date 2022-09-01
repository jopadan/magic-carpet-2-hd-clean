# Magic Carpet 2 HD

Magic Carpet 2 decompiled port project founded by [@turican0](https://github.com/turican0/) and maintained active fork by [@thobbsinteractive](https://github.com/thobbsinteractive/)\
including new HD assets.

## Dependencies

1. Operating system provided
   - [boost](https://github.com/boostorg/boost)
   - [boost-system](https://github.com/boostorg/system)
   - [libpng](https://github.com/glennrp/libpng)
   - [SDL2](https://github.com/libsdl-org/SDL)
   - [SDL2_image](https://github.com/libsdl-org/SDL_image)
   - [SDL-Mixer-X alias SDL2_mixer_ext](https://github.com/WohlSoft/SDL-Mixer-X)

2. Submodules included
   - [findfirst](https://github.com/MathieuTurcotte/findfirst)
   - [dirent](https://github.com/tronkko/dirent)
   - [inih](https://github.com/benhoyt/inih)
   - [itoa-benchmark](https://github.com/miloyip/itoa-benchmark)

3. Debug development optional
   - [clang-tidy](https://github.com/llvm/)
   - [sanitizers](https://github.com/sanitizers/)
   - [flatpak](https://flatpak.org/)

## Building

1. configure, compile and install
   - ```bash
     export BUILDTYPE=Debug # or Release
     mkdir -p build/${BUILDTYPE}
     cd build/${BUILDTYPE}
     cmake -DCMAKE_BUILD_TYPE=${BUILDTYPE} -DDATADIR=/usr/share/remc2 --install-prefix=/usr [SOURCE_DIR]
     make
     make install
     ```
  - optional components are enabled using the `-DUSE_SANITIZERS=True` and `-DUSE_CLANG_TIDY=True` flags

2. asset preparation
   - copy orginal CD contents to DATADIR/gamedisc

3. asset search path relative to
   - `cwd`
   - `$XDG_CONFIG_HOME/remc2`
   - `$HOME/.config/remc2`
   - CMakeList.txt: -DDATADIR=/usr/share/remc2 / config.ini: gameDataFolder=/usr/share/remc2
   - directory of remc2 executable

## Usage

  Run the `remc2` flatpak via
  ```bash
  flatpak run com.github.thobbsinteractive.magic-carpet-2-hd
  ```

