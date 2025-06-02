# ForceLoudnessEQ Guide
## *__First goal: Stop windows from auto-downloading drivers, as many effect audio enhancements.__*
If you want your audio enhancements, you'll need to change some windows settings first. This guide is for users who know how to update their drivers manually. If you are uncertain, I would err on the side of caution and not do this. You _**may**_ be able to skip to the second section and have this guide still work, but the audio effects may not be permanent/may not work at all.

Sidenote: on certain headsets, this will break the microphone's functionality. I may be able to find a fix but I am not entirely confident I will. If you use a seperate mic (not the one on your headset), this guide will not effect it.

1. Start by pressing WindowsKey+R, and type in "sysdm.cpl", then hit enter.

![image](https://github.com/user-attachments/assets/d457e956-052f-4308-9d19-629a90430cc7)

Click on the "Hardware" tab, then click "Device Installation Settings"

![image](https://github.com/user-attachments/assets/205ee0dd-8cdb-44c1-8ade-4ed49a78f2b1)

And in the window that pops up, ensure "No" is selected, and hit "Save Changes". You can close the sysdm.cpl window.

![image](https://github.com/user-attachments/assets/5ca72e45-2e5c-4eef-8c00-f055199e1712)

Note: This will prevent windows from updating your drivers, although this is required to prevent audio drivers from changing the registry after you change it.

2. Then, open run again with WindowsKey+R, and type in "gpedit.msc".

![image](https://github.com/user-attachments/assets/aef780b3-b62d-4cce-b4a8-773d974fe618)

On the right side, under "Computer Configuration", select "Adminitrative Templates" > "System" > "Device Installation" > "Device Installation Restrictions", and double click on "Prevent installation of devices not described by other policy settings". Enable the setting, click "Apply" then "OK".

![image](https://github.com/user-attachments/assets/0b5ca52b-92c4-47db-b3ce-837f2119c16e)

You can close out of the Group Policy window now. Note: You will need to remember the location of this setting if you plan on doing a lot of things, like plug in/out a thumb drive/keyboard/mouse, update any software, etc. If you keep things plugged in, it will be fine, but "installing" a device (regardless if the OS has seen it or not) will require a temporary disabling of this setting. You need to turn it on after, the sooner the better.

3. In the start menu, (aka, hit the windows key) and type in "Check for updates", and hit enter.

![image](https://github.com/user-attachments/assets/08a4bd93-22a5-4350-8250-4876d49c2917)

In the bottom-middle of the window, you'll see "Advanced options", click it.

![image](https://github.com/user-attachments/assets/2951212c-fccc-4737-851f-3e3c8bf33291)

In the Advanced options window, disable "Receive updates for other Microsoft products when you update Windows".

![image](https://github.com/user-attachments/assets/6c8cceaa-d374-45e4-996b-ce9697392206)

You can close this window now. This only effects Microsoft's apps that are in the store, as far as I know. (and it may redownload unwanted stuff like Dolby Audio)

4. Microsoft Store Settings

Open Microsoft Store by hitting the WindowsKey then type "Store". Open it, and in the top right, click your profile icon, then select settings.

![image](https://github.com/user-attachments/assets/f10ccc72-ea8b-427e-8b8d-154751e1dc63)

Ensure the top option, "App Updates", is disabled.

![image](https://github.com/user-attachments/assets/d4cb949d-8811-4cc0-a3e3-13770cc49694)

You can close the Microsoft Store now, but you are not done with it. You will need to run [this powershell script](Prevent_MS_Store_Updates.ps1) with administrator privileges to be able to fully disable Windows Store updates. Now, windows will not continue to download ANY software other than windows updates onto your PC.



## *__Second goal: Remove software that prevents audio effects from working.__*
Before you proceed, I'd recommend to disable (DO NOT REMOVE IT) the service "DeviceInstall" temporarily to ensure drivers do not get automatically installed during this process. You can do this in task manager. Once this goal is done, **re-enable this service!!!**

1. First things first, go to your Apps and Features tab in windows settings, and remove all apps that have the keywords "Realtek" "intelligo" "Dolby" "NGenuity". If you know of any other apps effecting your audio set up, I'd highly recommend removing those as well.

2. Now we must remove all of the programs that interfere with windows' default audio effects. On my PC, this would be: __Intel(R) Smart Sound Audio__ (all associated services and device drivers), __DolbyDAXAPI__ (service), __Dolby SWC Device__ (in device manager), __intelliGo Audio Processing Object__ (device manager), __HyperX NGenuity__, and __Realtek Audio Console__ (not the driver, the Store app. But I removed then reinstalled the driver as well when I made the registry edit work, so you may want to do that). 

To begin uninstalling them, you must stop their services first. They will automatically redownload themselves (most likely) if you do not. Do this by opening Task Manager and go to "Services" and look for any services that look like they are directly linked to those programs. There will be a lot of different services so do not just go disabling all of them, many are integral in having your computer work properly. It will be pretty obvious which services are related to what drivers.

A couple of services I distinctly remember disabling and removing: "DolbyDAXAPI" "RtkAudioUniversalService" "IgoAudioService".

After you disable them, you must remove them from your PC by opening powershell in administrator mode, then use the command `sc delete {service name}`. For example, to delete the DolbyDAX service, you would need to write `sc delete DolbyDAXAPI`.

Once you are done disabling and removing the services, you will need to go to device manager to delete the drivers associated with those services. Locations where these services will be installed inside of device manager are as follows: In "Audio inputs and outputs", "Audio Processing Objects (APOs)", "Other media devices", "Software components", "Sound, video and game controllers", and lastly "System devices". Ensure to delete the drivers off of your system if the prompts allow you to. You need to remove these devices to prevent them from redownloading the services that you just uninstalled. ENSURE you remove all of the related devices to
these services, or else you may have to repeat this "second goal" section again.

_**IF and only IF you skipped goal one, this would be a good time to check if the driver's will forcefully reisntall themselves anyway. I would restart your PC, and check task manager for those services / device manager for the devices. If they do, you'll most likely need to go through the instructions in goal one fully.**_



## *__Last goal: Install the correct drivers, and enable the effects.__*
If you got to this point, you now only need to switch your headphones' driver to windows default one (High Definition Audio Device) & edit the registry to be able to use the effects.

1. Go to WindowsKey+R, type in "mmsys.cpl", and right click the audio device that you wish to use loudness equalization on. Go to properties. Your screen should look something like this, except with different drivers. If these drivers are the ones you have (like, you have "Generic USB Audio") you can skip this step.

![image](https://github.com/user-attachments/assets/ba0ad896-caf9-48b0-a91f-8a7578534bc2)

Click on the properties tab in Controller Information, hit Change Settings, and click on Driver. 

![image](https://github.com/user-attachments/assets/3779e7a0-09e9-45c4-a6a5-a88e76fc714a)

Click update driver, Browse my computer for drivers, and "Let me pick from a list".

![image](https://github.com/user-attachments/assets/151b81f0-e8b7-45d2-8809-f2418324be03)

Uncheck "Show compatible hardware", and try to select either (Generic USB Audio)>"USB Audio Device"  OR  Microsoft>"High Definition Audio Device 10.0.19041.3636" (if you select this one, make sure you do not select the driver from 2001, that ones version ends in "19041.1").

![image](https://github.com/user-attachments/assets/43d4d6b6-32b7-4e94-9536-1180173aaa1d)

Select the one you want to try, and hit next. I forget how the driver installation works after that, but it will essentially either tell you that it worked and the device is working properly, or it is simply not working. If it doesn't work, try the other driver I recommended using the same process as shown above. 

Now that you have the correct driver installed, you can search for your device in the Registry. Open it, and go to this folder: 

`Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\MMDevices\Audio\Render`

It should look something like this:

![image](https://github.com/user-attachments/assets/2fab679e-6f65-44bc-8b38-8421d98716d4)

Now open these individual "keys" (they look like folders) and you will have to manually search in every `{XXXXXXXX-XXXX...}\Properties` and `{XXXXXXXX-XXXX...}\FxProperties` folder to find your specific headphones. There will be a LOT of unreadable data, but there may be a few lines that you can read, and that's how you'll find the correct key/folder to your device. (For clarity, the "Key" is {XXXXXXXX-XXXX...}.

For me, my folder looks like this:

![image](https://github.com/user-attachments/assets/253c1197-fd07-47b7-8dd9-fc673acf9cc0)

Now once you've found the right key, you can go to the `FxProperties` folder in that _**same**_ key, and find all of the strings that start with {d04e05a6-...}, as shown below.

![image](https://github.com/user-attachments/assets/d0bc936b-7032-49c5-bd3d-134e2c8db49b)

Note: the ends of the string will be different, and without going too deep into the details, they essentially represent different effect types (MFX, SFX, GFX, LFX). Windows default effects will always be set to these 3 strings, unless changed by the manufacturer:
```
{d04e05a6-594b-4fb6-a80d-01af5eed7d1d},3 = {5860E1C5-F95C-4a7a-8EC8-8AEF24F379A1}       # Enables effect tab GUI in mmsys.cpl
{d04e05a6-594b-4fb6-a80d-01af5eed7d1d},5 = {62dc1a93-ae24-464c-a43e-452f824c4250}       # This represents enabling either...
{d04e05a6-594b-4fb6-a80d-01af5eed7d1d},6 = {637c490d-eee3-4c0a-973f-371958802da2}       #Bass Boost or Loudness EQ, respectively. 
```
Note: I'm pretty confident you can ignore `{d04e05a6-594b-4fb6-a80d-01af5eed7d1d},0` as that only tells the OS what type of audio device you're using. Although if the effects don't work after finishing this step, you can set that value to `{DFF21CE2-F70F-11D0-B917-00A0C9223196}` to see if it works (it represents KS_NODETYPE_HEADPHONES if I recall correctly, you can check in C:\Windows\INF\wdma_usb.inf if you want to understand what these values represent).

Anyways: You must LOG then delete the `{d04e05...}` entries that end in anything except `{d04...},0`, `{d04...},3`, `{d04...},5`, `{d04...},6`. So, to be clear, that means the only strings that should be still there after you LOG and delete them are in the following screenshot (If the ,0/,3/,5/,6 values are not present in FxProperties, do not worry):

![image](https://github.com/user-attachments/assets/e0b2e5cf-b641-43f4-81e3-5a6c55520f49)

Now, you must add those strings manually to your system. If you already have a {d04...},3/,5/,6  , you should not add that same string twice (just modify the key to be set to the values that are in the code block). Once you are done with this, the following 3 strings should be in your FxProperties folder, with the values set to them (the strings and values are provided in the code block above):

![image](https://github.com/user-attachments/assets/43e27a87-17ba-47e2-96b0-095c6a2272d8)

Now, you can try to go to mmsys.cpl and enable the effect in your headphones' properties tab. I can't remember if I had to restart my PC after to fully set up these effects with your device, so if you notice the effect isn't applying correctly, try a system restart and then enable it again. If the effects tab disappears after a restart, you either: did not properly remove some app / driver / service that is effecting your devices' ability to keep the Registry changes (goal 2), you did not set up all of the proper Windows blocks to prevent it from auto-installing drivers/software (goal 1), you did not change your devices' default driver (start of goal 3), or your device is configured in a way that makes it impossible to use the default audio effects. I may work on a workaround for this soon, but that would be a couple-years-long project, so don't expect it soon.

If you have any issues, feel free to make an issue on this repo, although please follow the guide 1:1 before making any issue threads (do not skip any steps).

Hopefully this helped this get your effects back. If it did, star the repo to help get this fix to more people who need it.
