---
layout: post
title: Fix the Map Module in Lightroom 6
date: '2021-05-10 20:02'
tags:
 - Photography
---

In [my previous post]({{ site.baseurl }}{% link _posts/Photography/2021-05-09-my-photo-management-and-editing-workflow.md %}), I went over my photo management and editing workflow using the perpetual Lightroom 6. But since December 1, 2018, the Map functionality in Lightroom 6 stopped working. It's largely Google's fault than Adobe's fault, as Google decided to make breaking changes to Google Map's pricing model (read: price increase).

In early 2018, Google revamped the pricing model for embedding Google Maps into 3rd party applications, changing from free access or flat fees to [transaction based pricing](https://cloud.google.com/maps-platform/pricing/sheet/). The number of requests to the Google Maps APIs are counted, and after a threshold, a small fee is charged for every request.

Google's new pricing is not compatible with products that are licensed perpetually. With classic Lightroom, Adobe only got money once, but would have to pay Google each time you use the Map module. For Adobe, this is not a sustainable business model.

The Google Maps API key embedded in old versions of Adobe Lightroom expired on November 30, 2018.

To fix the Map module, we need to create our own API key and patch Lightroom to use it instead. My Lightroom is light so it will never exceed the free tier. [This GitHub repo](https://github.com/astuder/lightroom-map-fix) details the steps. This requires quite some IT skills so be cautious! I have successfully applied the patch and the Map module is functional again. I have not been charged a dime by Google by staying within the free tier.
