### SteamOS hardware enablement and development guidelines

If you want to create a Steam machine design, please keep the following things in mind when deciding on the specifications.

All hardware support is provided out of the box by the SteamOS image and as such, support for your devices has to be integrated into SteamOS ahead of time. This requires coordination between you, your device vendors and Valve well ahead of the release (or even specification) of your product.

#### Graphics

Graphics driver releases from the vendors are integrated as-is into SteamOS; as such it is your responsibility to coordinate with them to ensure the hardware and features you want to ship will be supported by their publicly available drivers by the time you're planning to start ramping up testing.

#### Picking hardware devices

When specifying your Steam Machine design, please make sure all the device vendors provide Linux support and are willing to work with Valve to integrate their support into the Linux kernel shipped with the current SteamOS release.

The kernel tree for SteamOS lives there:

https://github.com/ValveSoftware/steamos_kernel

This tree is the common kernel for all SteamOS devices; as such, be very careful about the scope of your patches and don't touch common code or interfaces unless you have a very good reason to do so.

The current shipping branch for SteamOS 1.0 is 'alchemist-3.10' and branched off 'linux-3.10.y' from kernel.org; send your pull requests against 'alchemist-3.10_proposed'.

Please coordinate with your vendors to confirm that support for their devices is either:
* Already present in that base version of the kernel, in which case it'll be supported out-of-the-box by the SteamOS kernel
* Supported in a subsequent release of the kernel, in which case they will have to be backported to kernel our tree (see above) and provided to us as a pull request that we can integrate with our kernel for testing.

Make sure your pull requests are fully tested on the target hardware or that Valve has access to the right hardware platform to confirm that the driver works.

We cannot support out-of-tree drivers; please ensure your vendors are ready to have all their changes upstream by the time they expect SteamOS testing to be able to begin.

#### Boot firmware display

The steps below will ensure we have all the tools at our disposal to offer a seamless boot experience from pressing the power button to the SteamOS login screen:

* The firmware should program the primary display device with its native timings, according to its EDID
** Not doing that will cause a modeswitch during boot, making the TV go dark for a few seconds and print mode information
* The firmware should allocate its EFI framebuffer with the same size as the mode it's programming
** The SteamOS bootloader and kernel inherit this framebuffer
** Allocating a mismatching framebuffer will cause a distorted/pixelated splash screen and flickering during boot
* The firmware should only display the SteamOS splash screen during regular boot
** The splash screen will be stored in EFI/steamos/splash.png by SteamOS and might be updated in-place in the future
** The splash screen should be upscaled or downscaled to the target framebuffer resolution (see above)
** The splash screen aspect ratio should be maintained
** The splash screen shouldn't be zoomed
** Nothing should be overlaid on top of the splash screen (eg. text prompts; "Press DEL to enter setup")
** The firmware shouldn't clear this splash screen before handing off to the SteamOS bootloader

#### Boot firmware input

* The BIOS setup screen should be able to be entered with the Tab key and navigated with the Arrow keys, Enter and Escape.
* The machine should be able to be powered on using a keyboard device by default, without having to enable it in the BIOS setup.
** Only the spacebar keycode should power on the system; other keys such as the arrow keys, Esc and Tab should not be able to power on the system
