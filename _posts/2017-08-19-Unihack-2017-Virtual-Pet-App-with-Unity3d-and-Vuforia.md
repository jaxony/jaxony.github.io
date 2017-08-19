---
layout: post
title: Unihack 2017 Virtual Pet App with Unity3d and Vuforia
---

![App](/images/woof/app.jpg)

For Unihack 2017, Team Woof made an augmented reality app in Unity. We wanted to do something that none of us had really touched before, as well as something that's pretty hot in the tech space right now. Augmented reality with Unity fit these criteria pretty well.

# App summary
Our mobile app allows users to interact with a virtual pet through voice commands and a joy stick. The motivation behind the app was to provide sick kids with a way to interact with animals wherever and whenever.

# Technologies
Check out our GitHub organisation [here](https://github.com/woofie).

- [Unity3d](https://unity3d.com/): Menu, UI, the whole app
- [Vuforia](https://www.vuforia.com/): A Unity plugin that handles all the augmented reality
- [Google Speech API](https://cloud.google.com/speech/): For voice recognition
- [Corgi model](https://www.assetstore.unity3d.com/en/#!/content/70082): We shelled out some cash to buy this cute little dog

# Features

We had grand plans for our virtual pet app at the beginning of the hackathon, but we quickly scaled down our features as the hackathon went on. To be fair, we took the healthy approach to Unihack: most of us went home and got a good night's sleep!

Our final app ended up with:

- **Voice commands** that trigger corresponding animations: e.g. "Hey Doggie, play dead!" causes the dog to fall on its side
- **Beautiful menu** with bespoke music and art designed by Albert Chen.

![Menu](/images/woof/menu.jpg){: .center-image}

- **Ball-chasing game** where the user moves the ball around and the pet chases after it, complete with dynamic speed and animation transitions.

<iframe width="420" height="315" src="https://youtube.com//embed/_-rcKmwVfJ4" frameborder="0" allowfullscreen></iframe>

# Improvements
## Voice commands
It was fairly hard to do voice recognition reliably with a dodgy phone mic and a noisy room. Simple buttons would be a practical addition to improving the user experience so that the dog would actually sit down when you want it to!

## Respawn
Judges who weren't used to AR would easily lose the dog by moving the camera view too far from the target image texture. The dog would then be lost in space... forever. A simple respawn script would be a good addition so that lost dogs are always found after `x` seconds :)

## Image Texture
We needed to use a pre-selected texture as our image target so that Vuforia could identify the orientation and position of the surface on which the dog would be rendered. I played with a bunch of different textures, from $5 to $10 notes to a Data61 promotional flyer that we picked up from the Unihack Careers Fair.

In an ideal world, we'd want Vuforia to find the plane that the camera is pointing at without the help of a pre-defined image texture. This generality would allow our app to function anywhere: in a hospital room, on a dinner table, etc.

I've been looking into AR more (for a chess recognition side project), and it seems like Apple's AR Kit provides this general plane-detection functionality. Check out a demo [here](https://blog.markdaws.net/arkit-by-example-part-2-plane-detection-visualization-10f05876d53).

# What we would do differently
Things got intense at the end, as hackathons do.

- Have a designated tester on the team.
- Test and build earlier
- Integrate components earlier
- Stay focused on the MVP


# The Team
- Xiao Liang
- Albert Chen
- Thomas Wang
- Martin Valentino
- Dennis Wirya
- Hayden Warmington
- Me