# anime4k-60fps
An attempt at a free and approachable guide to upscaling your anime and interpolating them on-the-fly to 60FPS. Yeah!

As you may already know, there are various solutions out there that help you interpolate on-the-fly to higher framerates. However most of them are paid solutions with limitations. Searching the web for how to do this yourself is a painstakingly hard effort if you have no experience and further still requires a long and tedious path to get right. And while these solutions do interpolation, they don't upscale. What if you want both? This is an attempt at a guide that allows you to upscale video using Anime4k and Interframe to get both upscaled video in glorious 60FPS.

This guide assumes x64 libraries for all programs and the use of a player capable of direct show, like MPC-HC.

Ready to get your toes wet? Let's dive in!

Requirements
------------
Should go without saying, but we'll state it still: Upscaling AND interpolation requires that you at least have some sort of modern hardware. For this testing we used a RTX 3060, but you may get away with less. Memory and bandwidth are the prime bottlenecks here, so anything under 6GB VRAM will have you struggle.

Install and get libraries
-------------------------
- Install mpc-hc x64 (This fork https://github.com/clsid2/mpc-hc)
- Install ffdshow x64 (https://sourceforge.net/projects/ffdshow-tryout/files/Official%20releases/64-bit/)
- Install Avisynth+ x64 (https://github.com/AviSynth/AviSynthPlus/releases/)
- Get madVR (http://madshi.net/madVR.zip)
- Get Anime4kCPP for Directshow (DSFilter) (https://github.com/TianZerL/Anime4KCPP/releases/tag/v2.5.0)
- Get InterFrame2.8.2 (https://www.spirton.com/interframe/)
- Make sure you have svp_flow_1_x64.dll
- Make sure you have svp_flow_2_x64.dll

If you want subs to work with Anime4kCPP, get AssFilter

Configuration
-------------------------
- MPC-hc -> Hit O -> Output -> madVR
- MPC-hc -> Hit O -> External Filters -> Add filter, add ffdshow raw video filter, Anime4KCPP (browse), AssFilter and madVR (browse)
- Set all these to prefer
- Ok
- Open a video
- Right click -> Filters -> FFDSHOW -> Check AviSynth and set filter in textbox to

```
SetMemoryMax(1024)
LoadPlugin("F:\Software\Video Interpolation\svpflow1_64.dll")
LoadPlugin("F:\Software\Video Interpolation\svpflow2_64.dll")
Import("F:\Software\Video Interpolation\InterFrame2.avsi")
SetFilterMTMode("FFVideoSource", 1)
ffdshow_source()
SetFilterMTMode("FFVideoSource", 2)
InterFrame(Preset="Fast", Tuning="Animation", GPU=true)
Cores=4
InterFrame(Preset="Fast", Tuning="Animation", GPU=true, Cores=Cores)
Prefetch(4)
```

Remember to replace paths of Interframe2.avsi, svpflow1_64.dll and svpflow2_64.dll to the correct paths.

Done
