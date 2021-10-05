![shingeki-no-kyojin](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fi.pinimg.com%2Foriginals%2Fbf%2F59%2Fba%2Fbf59bab0a7e667f437daec28fae065df.gif&f=1&nofb=1)

# Introduction
This is an ttempt and work in progress at a free and approachable way to upscaling your anime and interpolating them on-the-fly to 60FPS. Yeah!

As you may already know, there are various solutions out there that help you interpolate on-the-fly to higher framerates. However most of them are paid solutions with limitations. Searching the web for how to do this yourself is a painstakingly hard effort if you have no experience and further still requires a long and tedious path to get right. And while these solutions do interpolation, they don't upscale. What if you want both? This is an attempt at a guide that allows you to upscale video using Anime4k and Interframe to get both upscaled video in glorious 60FPS.

This guide assumes x64 libraries for all programs and the use of a player capable of direct show, like MPC-HC.

Ready to get your toes wet? Let's dive in!

## Requirements
------------
Should go without saying, but we'll state it still: Upscaling AND interpolation requires that you at least have some sort of modern hardware. For this testing we used a RTX 3060, but you may get away with less. Memory and bandwidth are the prime bottlenecks here, so anything under 6GB VRAM will have you struggle.

## Install and get libraries
-------------------------
- Install mpc-hc x64 (This fork https://github.com/clsid2/mpc-hc)
- Install ffdshow x64 (https://sourceforge.net/projects/ffdshow-tryout/files/Official%20releases/64-bit/)
- Install Avisynth+ x64 (https://github.com/AviSynth/AviSynthPlus/releases/)
- Get madVR (http://madshi.net/madVR.zip)
- Get Anime4kCPP for Directshow (DSFilter) (https://github.com/TianZerL/Anime4KCPP/releases/)
- Get InterFrame2.8.2 (https://www.spirton.com/interframe/) (A copy can be found in the libraries folder)

Next you'll need SVP flow. These are based off MVTools 2.5. You'll need the correct version for them to work (4.2.0.142), since this version doesn't require SVP Manager
- Get svp_flow_1_x64.dll 4.2.0.142 (http://avisynth.nl/index.php/SVPflow) (A copy can be found in the libraries folder)
- Make sure you have svp_flow_2_x64.dll 4.2.0.142 (http://avisynth.nl/index.php/SVPflow) (A copy can be found in the libraries folder)

If you want subs to work with Anime4kCPP, get AssFilter

## Configuration
-------------------------
After all of these are installed and downloaded, you need to add them to MPC-HC. First off we need to add the external filters for FFDShow Raw Video Filter, Anime4KCPP, AssFilter and madVR
- MPC-hc -> Hit O -> External Filters -> Add filter, add ffdshow raw video filter, Anime4KCPP (browse), AssFilter and madVR (browse). See all to Prefer.
- ![image](https://user-images.githubusercontent.com/706874/136077969-9f9bb239-4f93-4359-abf0-b9ee997fc0c7.png)

Next, head over to the Output to madVR. If you need subtitle support, choose AssFilter in Subtitle Renderer.
- MPC-hc -> Hit O -> Output -> madVR
![image](https://user-images.githubusercontent.com/706874/136078160-83fe22fd-5544-45e0-8281-365e52dc0eec.png)


- Set all these filters to prefer
- Ok
- Open a video
- Right click video frame -> Filters -> ffdshow raw video filter

![image](https://user-images.githubusercontent.com/706874/136078286-fd32d3fa-947e-490c-b6c9-c3b5c2a3727c.png)

 In the new window -> Check AviSynth and set filter in textbox to

```
SetMemoryMax(1024)
LoadPlugin("YOUR\PATH\TO\svpflow1_64.dll")
LoadPlugin("YOUR\PATH\TO\svpflow2_64.dll")
Import("YOUR\PATH\TO\InterFrame2.avsi")
SetFilterMTMode("FFVideoSource", 1)
ffdshow_source()
SetFilterMTMode("FFVideoSource", 2)
InterFrame(Preset="Fast", Tuning="Animation", GPU=true)
Cores=4
InterFrame(Preset="Fast", Tuning="Animation", GPU=true, Cores=Cores)
Prefetch(4)
```

![image](https://user-images.githubusercontent.com/706874/136078534-f07bc527-ab65-4b67-a5f7-af4672cd20d2.png)

Remember to replace paths of Interframe2.avsi, svpflow1_64.dll and svpflow2_64.dll to the correct paths.

Verify that FFDShow is running by looking in your task bar
![image](https://user-images.githubusercontent.com/706874/136078760-2dec18b1-9410-4948-8daa-8540ac1d983d.png)

Enjoy! Your video should be both upscaled and running way smoother! Your apartment might also get toastier as an added bonus.
