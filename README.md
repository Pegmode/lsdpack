# lsdpack

Records LSDj songs for use in stand-alone Game Boy ROMs. (E.g. your own games, demos, music albums...)

## How Does It Work?

lsdpack plays back LSDj songs using an emulated Game Boy Color and records direct writes to the sound chip. Those recordings can then be played back from another ROM. This player is very fast and can easily play back songs that would normally choke a Game Boy Classic. The drawback with this approach is that recordings grow big - a song can easily take a megabyte or more.

## Building

Requires CMake and a C++ compiler. Exact build steps are platform dependent - see [Running CMake](https://cmake.org/runningcmake/)

## Recording Songs

All songs in the .sav must first be prepared so that they are eventually stopped with the HFF command. Then, place your .sav and .gb file in the same directory and run e.g. `./lsdpack.exe lsdj.gb` to record the songs to `lsdj.s`.

## Playing Songs from Your Own Code

An example Game Boy player ROM can be built using RGBDS:

    rgbasm -o boot.o boot.s
    rgbasm -o player.o player.s
    rgbasm -o lsdj.o lsdj.s
    rgblink -o player.gb boot.o player.o lsdj.o
    rgbfix -v -m 0x19 -p 0 player.gb

### boot.s

An example for how to call the player. Replace with
your own game, music selector or whatever you feel like :)

### player.s

Contains the player code. Following functions are exported:

    ; IN: a = song number
    ; OUT: -
    ; SIDE EFFECTS: trashes de, hl
    ;
    ; Starts playing a song. If a song is already playing,
    ; make sure interrupts are disabled when calling this.
    ;
    LsdjPlaySong::

    ; IN: -
    ; OUT: -
    ; SIDE EFFECTS: changes ROM bank
    ;
    ; Call this six times per screen update,
    ; evenly spread out over the screen.
    ;
    LsdjTick::
