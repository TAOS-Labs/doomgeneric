# Pranay's instructions

This repository will provide you the doomgeneric executable in the doomgeneric/doomgeneric folder, along with doom1.wad which the
game can't boot without. The makefile expects that the file doomgeneric_xlib.c is the one that contains the main function along with
all the functions that we need to write ourselves to integrate with our frame buffer. You will see that everything is commented out
because it is all dependent on X11, and I ripped it out so we can write our own integration code. Here are examples of other integrations
that might be helpful. If you want to recompile doom, you first need to use the setup_sysroot script that I provided, run make in the
doomgeneric/doomgeneric folder, and then to test in the kernel, drop the executable into the resources/executables folder, and rerun
make blank_drive. You can take a look at the test_simple_c_ret testcase to know how to load and run executables from the sd card.
Keep in mind it'll likely take close to a minute to load.

The PincerOS's implementation of this (in Rust)
https://github.com/pincerOS/kernel/blob/doom-testing/crates/doomgeneric/src/main.rs

The SDL port, which should be relatively clear in the expected behavior of the functions
https://github.com/ozkl/doomgeneric/blob/master/doomgeneric/doomgeneric_sdl.c



# doomgeneric
The purpose of doomgeneric is to make porting Doom easier.
Of course Doom is already portable but with doomgeneric it is possible with just a few functions.

To try it you will need a WAD file (game data). If you don't own the game, shareware version is freely available (doom1.wad).

# porting
Create a file named doomgeneric_yourplatform.c and just implement these functions to suit your platform.
* DG_Init
* DG_DrawFrame
* DG_SleepMs
* DG_GetTicksMs
* DG_GetKey

|Functions            |Description|
|---------------------|-----------|
|DG_Init              |Initialize your platfrom (create window, framebuffer, etc...).
|DG_DrawFrame         |Frame is ready in DG_ScreenBuffer. Copy it to your platform's screen.
|DG_SleepMs           |Sleep in milliseconds.
|DG_GetTicksMs        |The ticks passed since launch in milliseconds.
|DG_GetKey            |Provide keyboard events.
|DG_SetWindowTitle    |Not required. This is for setting the window title as Doom sets this from WAD file.

### main loop
At start, call doomgeneric_Create().

In a loop, call doomgeneric_Tick().

In simplest form:
```
int main(int argc, char **argv)
{
    doomgeneric_Create(argc, argv);

    while (1)
    {
        doomgeneric_Tick();
    }

    return 0;
}
```

# sound
Sound is much harder to implement! If you need sound, take a look at SDL port. It fully supports sound and music! Where to start? Define FEATURE_SOUND, assign DG_sound_module and DG_music_module.

# platforms
Ported platforms include Windows, X11, SDL, emscripten. Just look at (doomgeneric_win.c, doomgeneric_xlib.c, doomgeneric_sdl.c).
Makefiles provided for each platform.

## emscripten
You can try it directly here:
https://ozkl.github.io/doomgeneric/

emscripten port is based on SDL port, so it supports sound and music! For music, timidity backend is used.

## Windows
![Windows](screenshots/windows.png)

## X11 - Ubuntu
![Ubuntu](screenshots/ubuntu.png)

## X11 - FreeBSD
![FreeBSD](screenshots/freebsd.png)

## SDL
![SDL](screenshots/sdl.png)
