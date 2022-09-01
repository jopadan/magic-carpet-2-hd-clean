# Magic Carpet 2 HD

Magic Carpet 2 decompiled port project founded by [@turican0](https://github.com/turican0/) and maintained active fork by [@thobbsinteractive](https://github.com/thobbsinteractive/)\
including new HD assets.

## Dependencies

- [boost](https://github.com/boostorg/boost)
- [boost-system](https://github.com/boostorg/system)
- [libpng](https://github.com/glennrp/libpng)
- [SDL2](https://github.com/libsdl-org/SDL)
- [SDL2_image](https://github.com/libsdl-org/SDL_image)
- [SDL-Mixer-X alias SDL2_mixer_ext](https://github.com/WohlSoft/SDL-Mixer-X)

### Submodules

- [findfirst](https://github.com/MathieuTurcotte/findfirst)
- [dirent](https://github.com/tronkko/dirent)
- [benhoyt](https://github.com/benhoyt/inih)
- [itoa-benchmark](https://github.com/miloyip/itoa-benchmark)

### Debug

- [clang-tidy](https://github.com/llvm/)
- [sanitizers](https://github.com/sanitizers/)
- [flatpak](https://flatpak.org/)

## Building

- configure, compile and install

  ```bash
  export BUILDTYPE=Debug # or Release
  mkdir -p build/${BUILDTYPE}
  cd build/${BUILDTYPE}
  cmake -DCMAKE_BUILD_TYPE=${BUILDTYPE} -DDATADIR=/usr/share/remc2 --install-prefix=/usr [SOURCE_DIR]
  make
  make install
  ```
  optional components are enabled using the

  `-DUSE_SANITIZERS=True`

  and

  `-DUSE_CLANG_TIDY=True`

  flags

- asset preparation

  1. copy orginal CD contents to DATADIR/gamedisc

- asset search path relative to

  1. current working dir
  2. `$XDG_CONFIG_HOME/remc2`
  3. `$HOME/.config/remc2`
  4. DATADIR=/usr/share/remc2
  5. dir of executable

## Usage

  Run the `remc2` flatpak via
  ```bash
  flatpak run com.github.thobbsinteractive.magic-carpet-2-hd
  ```

