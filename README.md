# ForceLoudnessEQ
Sidenote: I created [this youtube video](https://www.youtube.com/watch?v=WmdMPfWAci0) and [this comprehensive guide](guide.md), and if your main goal is to get Windows default audio effects back, you can follow either one of those to accomplish that.

Force Loudness EQ for outputs that support enhancements - please note, this will disable all software audio enhancements/effects made by the headsets manufacturer (like DTS:X).

After having my own problems with enabling Loudness EQ, I decided to make this guide for those trying to implement it themselves. But, I figure I should start out with the basics. Skip to the "guide.md" page if you only want the solution.

# Why are enhancements auto-enabled with some audio drivers and not others?
Certain audio drivers may reference their own sAPO's (system effect APOs). When windows installs an audio device, it checks to see if Windows should attempt to use a default audio driver, or a custom one made by the manufacturer. This allows headphone manufacturers to implement, for example, DTS:X audio effects. The main idea of this project is to hijack that ability to check for a manufacturer driver/sAPO, and instead force it to use the Windows default driver and effect APO. 
These effects are enabled in your headphone drivers' registry folder, located at

`Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\MMDevices\Audio\Render\{XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX}\FxProperties`

with your headphones' id in place of {XXX...}. If you don't know your headphones ID, go to `~\Audio\Render`, and open each key, looking in the "Properties" folder for indications that you are looking at your headphones' key (for example, my headphones had the strings "HyperX Cloud II Wireless" and "DTS Audio Effects for HyperX Relay headset" in `~\Properties` and `~\FxProperties`, respectively). There may be multiple entries for a single device as well, as if you reinstall a driver a new Registry key will be made for that new installation, so look through ALL of the keys regardless if you "already found the folder".

In this `{XX...}` key, you will see that inside of `~\FxProperties`, there will be a few different keys that start with the string `{d04e05...}`, the letters may be capitalized. They may also not exist at all yet, as each manufacturer has their own way to implement these effects, shown inside of a `.inf` file that you can find in a multitude of ways. The most common approach is to use the values `{d04e05...},5` and `{d04e05...},6` to set the sAPO, although your headset may use `{d04e05...},13` and `{d04e05...},14` like my own. By replacing the CLSID that this key points to, you can force Windows to use its own audio effects, including Bass Boost and Loudness Equalization, on your audio device. It is important to note though, you may need to use a Windows default audio driver with this to allow audio to be sent properly to the device, and there is more information on that in the guides I created.


# Why does this work?
When Windows installs any device, it checks if the manufacturer provided a file to install that device "fully properly" within Windows. For example, this means that a manufacturer can set up their sAPO effects to work how they want it to, even if you would prefer to use Windows default audio effects. By changing the driver to one that is compatible with Windows default audio effects,  then implementing the correct CLSID pointer to Windows audio effects, and finally stopping Windows from updating that device without your permission, you can have it so the device has access to these wonderful audio effects that prevent ear pain, increase the quality of music for some, etc.

You may also ask, why can't I simply layer these audio effects on top of each other? Unfortunately, this is because Windows audio effects are classified as GFX and LFX effects, so they could be overwritten by any other audio effect (SFX, MFX, and EFX effects). You need to force Windows to use it's own audio effects on that device, even though it is a time-consuming process.

Good Luck, and follow the guide to a tee.


Referenced sources:

[Falcosc's powershell script](https://github.com/Falcosc/enable-loudness-equalisation)

Unreferenced sources:

[dechamp's windows APO guide](https://github.com/dechamps/APO) ; wdma_usb.inf (in C:\Windows\INF) ; oem61.inf (hyperX's OEM INF)
