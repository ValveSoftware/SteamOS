### SteamOS hardware enablement and development guidelines

If you want to create a Steam machine design, please keep the following things in mind when deciding on the specifications.

All hardware support is provided out of the box by the SteamOS image and as such, support for your devices has to be integrated into SteamOS ahead of time. This requires coordination between you, your device vendors and Valve well ahead of the release (or even specification) of your product.

### General Design Considerations

Beyond specific component choices, OEMs should consider the following principles, drawing inspiration from Valve's own hardware efforts (like the Steam Deck), to create a successful Steam Machine:

*   **Console-like Experience:** Strive for ease of use, quick boot/resume times, and seamless controller navigation within SteamOS. Reliable suspend-and-resume functionality is highly desirable and requires careful hardware and driver selection.
*   **Optimized for Gaming:** Prioritize components and system tuning that deliver a smooth gaming experience. This includes considering integrated graphics solutions (e.g., APUs from vendors like AMD or Intel) for balanced performance and power efficiency, especially for compact or portable designs.
*   **Power Efficiency:** Whether for portable or stationary devices, power efficiency is key to reducing heat, fan noise, and energy consumption. Component selection (e.g., low-power RAM, efficient power regulation) and firmware-level power management features (e.g., ACPI states, CPU frequency scaling) should reflect this.
*   **Thermal Management:** Especially in Small Form Factor (SFF) designs common for living room use or portable devices, robust thermal solutions are critical to maintain performance and system longevity without excessive noise. This includes heatsinks, fan profiles, and chassis airflow.
*   **Input Device Support:** Ensure broad compatibility with popular game controllers (e.g., Steam Controller, Xbox controllers, PlayStation controllers, and other XInput/DirectInput compatible devices) via both USB and Bluetooth. Integrated controls, if part of the design, must have excellent Linux support and ideally use standard kernel interfaces.
*   **Display and Audio Quality:** For an immersive gaming experience, ensure high-quality display outputs (with consideration for low latency panels, and potentially VRR support for external displays if the GPU supports it). Audio capabilities should include clean analog outputs and robust digital audio via HDMI/DisplayPort. Support for modern Bluetooth audio codecs (e.g., AptX, LDAC) can enhance the user experience with wireless peripherals.
*   **Storage for Gaming:** While covered under "Modern Hardware Standards," reiterate that fast, responsive storage (preferably NVMe) is crucial for game load times and overall system responsiveness.

#### Graphics

Graphics driver releases from the vendors are integrated as-is into SteamOS; as such it is your responsibility to coordinate with them to ensure the hardware and features you want to ship will be supported by their publicly available drivers by the time you're planning to start ramping up testing.

#### Picking hardware devices

When specifying your Steam Machine design, please make sure all the device vendors provide Linux support and are willing to work with Valve to integrate their support into the Linux kernel shipped with the current SteamOS release.

The kernel tree for SteamOS lives there:

https://github.com/ValveSoftware/steamos_kernel

This tree is the common kernel for all SteamOS devices. OEMs should coordinate closely with Valve regarding the appropriate kernel version and branch for new hardware projects. SteamOS has evolved, and current versions utilize much more recent Linux kernels (e.g., based on Linux 5.13, 6.1, or newer, as seen in branches like `jupiter-5.13`, `neptune-6.1`, and other actively maintained branches). It is crucial to target a Valve-supported branch for any new hardware.

As such, be very careful about the scope of your patches and don't touch common code or interfaces unless you have a very good reason and have coordinated this with Valve.

Please coordinate with your vendors to confirm that support for their devices is either:
* Already present in the target base version of the kernel identified in coordination with Valve, in which case it'll likely be supported out-of-the-box by the SteamOS kernel.
* Supported in a subsequent release of the kernel, in which case they will have to be backported to the relevant SteamOS kernel tree (see above) and provided to us as a pull request that we can integrate for testing.

Make sure your pull requests are fully tested on the target hardware or that Valve has access to the right hardware platform to confirm that the driver works.

We cannot support out-of-tree drivers; please ensure your vendors are ready to have all their changes upstream by the time they expect SteamOS testing to be able to begin.

**Modern Hardware Standards:**

When selecting components, OEMs should prioritize hardware with robust, mainline Linux support. Consideration should be given to the following modern standards:

*   **Wireless Networking (Wi-Fi):**
    *   Aim for chipsets supporting Wi-Fi 6 (IEEE 802.11ax) or Wi-Fi 6E for improved performance, capacity, and efficiency, especially in dense environments.
    *   Ensure chipsets have stable, well-maintained Linux drivers.
    *   Support for WPA3 security is highly recommended.
*   **USB Connectivity:**
    *   Incorporate USB Type-C ports where appropriate, supporting features like USB 3.2 (Gen 2 or Gen 2x2) or USB4 for high-speed data transfer.
    *   For devices intended for charging or docking, USB Power Delivery (PD) support is essential.
    *   If video output via USB-C is a feature, ensure DisplayPort Alt Mode compatibility and thorough testing on Linux.
*   **Storage:**
    *   Primary storage should ideally be NVMe-based (e.g., M.2 PCIe SSDs) for optimal OS and game loading performance. Consider current PCIe generations (e.g., PCIe 4.0 or newer) for maximum throughput.
    *   Verify Linux compatibility for the chosen NVMe controller and firmware. While most are well-supported, diligence is required.
    *   Adequate thermal management for NVMe SSDs should be implemented, especially in compact form factors.
*   **Bluetooth:**
    *   Utilize Bluetooth 5.0 or newer for improved range, speed, and features like LE Audio (if relevant to the product).
    *   Confirm solid Linux driver support for the Bluetooth module.

OEMs are encouraged to discuss their specific component choices with Valve to ensure optimal compatibility and performance with SteamOS.

#### Boot firmware display

The steps below will ensure we have all the tools at our disposal to offer a seamless boot experience from pressing the power button to the SteamOS login screen:

* The firmware should program the primary display device with its native timings, according to its EDID
 * Not doing that will cause a modeswitch during boot, making the TV go dark for a few seconds and print mode information
* The firmware should allocate its EFI framebuffer with the same size as the mode it's programming
 * The SteamOS bootloader and kernel inherit this framebuffer
 * Allocating a mismatching framebuffer will cause a distorted/pixelated splash screen and flickering during boot
* The firmware should only display the SteamOS splash screen during regular boot
 * The splash screen path and update mechanism (e.g., potentially EFI/steamos/steamos.png or other paths) should be confirmed with Valve, as it might be updated in-place in the future.
 * The splash screen should be upscaled or downscaled to the target framebuffer resolution (see above)
 * The splash screen aspect ratio should be maintained
 * The splash screen shouldn't be zoomed
 * Nothing should be overlaid on top of the splash screen (eg. text prompts; "Press DEL to enter setup")
 * The firmware shouldn't clear this splash screen before handing off to the SteamOS bootloader

#### Boot firmware input

* The machine needs to be able to be woken up and powered on from S5 via USB power events from ports intended for primary input devices (e.g., game controllers, keyboards). This includes support for common wireless dongles and direct USB connections.
* The BIOS setup screen should be able to be entered with the Tab key (or other standard keys like Del, F2, etc., clearly indicated to the user if non-standard) and navigated with the Arrow keys, Enter, and Escape.

#### Factory Install Processes

* The machine's RTC should be set to UTC time, not local time. The end user selects their time zone during the first boot. 
