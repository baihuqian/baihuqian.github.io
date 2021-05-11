---
layout: post
title: Import New Lens and Camera Profile into Lightroom 6
date: '2021-05-10 19:53'
tags:
 - Photography
---

In [my previous post]({{ site.baseurl }}{% link _posts/Photography/2021-05-09-my-photo-management-and-editing-workflow.md %}), I went over my photo management and editing workflow using the perpetual Lightroom 6. However, it is not longer updated with newer lens and cameras. While Sony FE lenses can bake lens corrections into the RAW file, as do the Tamron FE trinity, it would be nice to get new camera and lens profile into Lightroom itself.

Adobe used to provide a software utility called Adobe lens profile downloader, written in its AIR technology. But it has since been discontinued along with AIR

Luckily, Adobe gives away new lens and camera profiles for free via [the DNG converter](https://helpx.adobe.com/photoshop/using/adobe-dng-converter.html). DNG converter's profiles are stored in Camera Raw/Photoshop location while Lightroom has its own location, thus updating DNG converter does not automatically apply new profiles to Lightroom. However, you can always copy Adobe lens profiles from newer versions of the DNG Converter into the user profile area of Lightroom, and they will be seen by Lightroom as if they were profiles you created or downloaded from a third-party.

For Windows user, after installing the latest DNG Converter, you can find its profiles at:
```
C:\ProgramData\Adobe\CameraRaw\LensProfiles\1.0
```

They are organized by manufacturers. You can copy the specific `.lcp` files that match the lens you want support for into an area under your user library area:

```
%APPDATA%\Adobe\CameraRaw\LensProfiles\1.0\
```
`%APPDATA% `is a shortcut for a hidden path:
```
C:\Users\*yourusername*\AppData\Roaming\
```
In case you can't see hidden data, change the options in Explorer and the full path is the following:

```
C:\Users\*yourusername*\AppData\Roaming\Adobe\CameraRaw\LensProfiles\1.0\
```

You should be able to find the new profiles in Lightroom now.
