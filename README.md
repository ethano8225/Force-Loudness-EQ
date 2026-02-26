# ForceLoudnessEQ
Sidenote: I created [this youtube video](https://www.youtube.com/watch?v=WmdMPfWAci0) and [this comprehensive guide](guide.md). If your main goal is to get Windows default audio effects back, you can follow either of those guides to accomplish that.

Force Loudness EQ for outputs that support enhancements - note, this will disable all software audio enhancements/effects made by the headsets manufacturer (like DTS:X).

This section gives a background on system effect APOs (sAPOs). Skip to "guide.md" if you only care for the solution.

# Why are enhancements auto-enabled with some audio drivers and not others?
Audio drivers may reference their own sAPO's. When windows installs an audio device, it checks to see if Windows should attempt to use the default audio effects, or custom ones created by the manufacturer. This allows headphone manufacturers to implement, for example, DTS:X audio effects. The main idea of this project is to prevent Windows' ability to check for a manufacturer-provided driver/sAPO, and force it to use the Windows default driver and effect APO. 
These effects are enabled in your headphone drivers' registry folder, located inside of Windows' registry at

`Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\MMDevices\Audio\Render\{XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX}\FxProperties`

with your headphones' id in place of {XXX...}. If you don't know your headphones ID, go to `~\Audio\Render`, open each key, looking in the "Properties" folder for indications that you are looking at your headphones' key (for example, my headphones had the strings "HyperX Cloud II Wireless" and "DTS Audio Effects for HyperX Relay headset" in `~\Properties` and `~\FxProperties`, respectively). There may be multiple entries for a single device as well. If you reinstall a driver a new Registry key will be made for that new installation, so look through ALL of the given keys in case more than one represents your headset.

In this `{XX...}` key, you will see that inside of `~\FxProperties`, there will be a few different keys that start with the string `{d04e05...}`, the letters may be capitalized. They may also not exist at all yet, as each manufacturer has their own way to implement these effects. Typically the effect APO information is shown inside of a `.inf` file that you can find in a multitude of ways. Either way, the most common approach is to use the values `{d04e05...},5` and `{d04e05...},6` to set the sAPO, although your headset may use `{d04e05...},13` and `{d04e05...},14` like my own. By replacing the class identifier (CLSID) that this key points to, you can force Windows to use its own audio effects, including Bass Boost and Loudness Equalization, on your audio device. It is important to note though, you may need to use a Windows default audio driver with this. Certain manufacturers, i.e. Intel and their "Smart" audio driver disables the Windows' effects and prevents them from working properly.


# Why does this work?
When Windows installs any device, it checks if the manufacturer provided a file to install that device properly within Windows. For example, this means that a manufacturer can set up their sAPO effects to work how they want it to, even if you would prefer to use Windows default audio effects. By changing the driver to one that is compatible with Windows default audio effects, then implementing the correct CLSID pointer to Windows audio effects, and finally stopping Windows from updating that device automatically, you can have it so the device has access to these wonderful audio effects that prevent hearing loss, increase the quality of music for some, etc.

You may also ask, why can't I simply layer these audio effects on top of each other? Unfortunately, this is because Windows audio effects are classified as GFX and LFX effects, so they could be overwritten by any other audio effect (SFX, MFX, and EFX effects). You need to force Windows to use its own audio effects on that device, even though it is a time-consuming process.

Good luck, and follow the guide.


Referenced sources:

[Falcosc's powershell script](https://github.com/Falcosc/enable-loudness-equalisation)

Unreferenced sources:

[dechamp's windows APO guide](https://github.com/dechamps/APO) ; wdma_usb.inf (in C:\Windows\INF) ; oem61.inf (hyperX's OEM INF)
