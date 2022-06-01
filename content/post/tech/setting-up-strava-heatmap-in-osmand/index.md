---
title: "Setting up Strava Heatmap in OsmAnd"
date: 2019-11-28T12:59:10+01:00
draft: true
image: screenshot.png
---


OsmAnd is a great app for hiking, mountain biking, or other outdoorsy things, however, one can make it even better by underlaying (or overlaying) data from Strava’s heatmap in addition to the offline Openstreetmap data. This is quite useful for spotting potential trails or routes that are not in Openstreetmap yet, or for identifying how “good” (poular…) a trail might be, based on how many Strava users take it. Here’s what it looks like:

Strava heatmap as an underaly in Osmand

How to set it up; the easy way:

(This uses an online proxy to retrive the map tiles from Strava, so you don’t have to login, fiddle around with session tokens in the URL, etc. yourself) You can find more details about it here, including other maps available.

In OsmAnd, go to “Configure map”, then “Map source…”, “Define/Edit…”, and enter the following details:

    Name: Strava heatmap (or anything you want)

    URL: https://anygis.ru/api/v1/Tracks_Strava_All_HD/{1}/{2}/{0}

    Maximum zoom: 15

    Minimum zoom: 0

That’s it. Just enable “underlay map”, and set it to “Strava heatmap”. If it doesn’t work for you, read on:

And in case the easy way doesn’t work (you’ll need a Strava account and a desktop computer):

Go to https://www.strava.com/heatmap, login, and then open your browser’s developer tools (hit F12). You’ll need to go to the storage tab (in chrome, that’s under “Application” -> “Cookies”), and copy the values of CloudFront-Signature, CloudFront-Key-Pair-Id and CloudFront-Policy, to make up the following URL: (replace the dots with the values of those cookies…)

https://heatmap-external-a.strava.com/tiles-auth/all/gray/{0}/{1}/{2}.png?px=256&Signature=.......&Key-Pair-Id=.......&Policy=.......

Send this to your phone somehow once you’ve got it all.

In OsmAnd, go to “Configure map”, then “Map source…”, “Define/Edit…”, and enter the following details:

    Name: Strava heatmap (or anything you want)

    URL: the url you got earlier.

    Maximum zoom: 16

It should look something like this:

Strava heatmap as an underaly in Osmand

After that, just enable “underlay map”, and set it to “Strava heatmap”. You might also need to set “map source” back to “Offline vector maps” if OsmAnd changes to only the heatmap.

If it stops working; Strava probably logged you out, you’ll need to do the whole URL assembly-copy-paste thing again - but you can do “select existing” in the OsmAnd “Define/edit” dialog to just update the URL.

Have fun!
