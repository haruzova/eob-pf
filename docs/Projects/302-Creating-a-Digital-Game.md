# 302 - Creating a Digital Game
### <font color="#7359b3">Final Product</font>
[Gameplay Recording](https://drive.google.com/file/d/199mIUALFtldU87H-SbGBhyG4ghTByWn4/view?usp=drivesdk)
(haven't figured out video embeds with mkdocs yet so just click the link and pretend this image is the thumbnail)
![](20240503-godot-idk.png)
### <font color="#7359b3">Software and tools used</font>

| **Game engine:**      | Godot 4.1 - 4.2                          |
| --------------------- | ---------------------------------------- |
| **Pixel art assets:** | Aseprite, Libresprite, Clip Studio Paint |
| **Animated sprites:** | Clip Studio Paint, Libresprite           |

### <font color="#7359b3">Self Evaluation</font>
***What is the project you have been working on this term?***

- 2D Platformer Game in Godot

***Do you think you successfully did this?***

- Yes. Mostly.

***What parts of your project do you like?***

- I like the little shrimpy clouds in the background and the player character's idle animation because they're cute and I think I did an okay job with them.

***What could you improve on, and what part would you do differently next time?***

- I could have definitely been a lot more organised in how I actually got everything working, as well as not forgetting to add audio... Next time, the first thing I'd do would be to start off using asset packs in order to get the game figured out, and work out what sprites I want to make afterwards. It slowed things down a lot (and demotivated me because UGHHH tile creation)

***What part of the work did you find the most difficult and why?***

- Making pixel art, tilesets especially, was really hard,because I'm not used to using colour so much, or doing the maths you need to do for tiling assets. Even if it's just basic +, - , x, / stuff... It's hard...

---- 
### <font color="#7359b3">Creating pixel item sprites</font>
##### 08/09/23
![bubger](assets/Devlog/302/bubgerwip.png)
![burger](bubger.png)

I made a BUBGER in ASEPRITE !!!!!!!!!!!!!!!!!!
this is IMPORTANT I think (spoilers: it's not important)
##### 12/09/23
![ghost](gohtkorewip.webp)

![ghost fin](goht.png)

I made a GOHST in ASEPRITE and CSP

AND CAKE !!!!!!!! 
![cake](cakereal.png)

### <font color="#7359b3">Character design and sprites</font>
##### 25/09/23
![wip sprite](20230925-cspanim.png)

![](wipbingusidle.gif)

![](wipbinguswalk.gif)
Starting to create character sprite sheets for a 2D platformer in clip studio paint. I was possessed by the spirit of glorble gloopus and he did all the work for me. He's not very good at it huh
##### 26/09/23
![](idlelightweb.gif)

![](Spritesheet.png)
Continuing to work on sprites and designing the player character (her name is bingus and she is a single celled organism)
### <font color="#7359b3">Game environment</font> (02/10/23)
##### 02/10/23
![](20230929-162642_Godot.png)
Making a start on the 2D platformer game in Unreal Engine, except I actually did it like last week and it's in Godot instead because I like it more. Sorry
It's using the Sunnyland asset pack here, but i don't like it much and they don't look good with any of my sprite styles so they will be replaced and tossed into the abyss. I foresee absolutlely no undesirable consequences
![](20230929-Godot.png)
Also godot 4.1 has a bug on Android where it'll throw up these errors every time you save ANYTHING for no reason. She's so crazyyyy Love her !!!

### Making a Background
##### 09/10/23
![](20231006-230010_Godot.png)
Drawing and adding a background designed to work with parallax (the trees and sky are separate layers)

### Distractions & HUD stuff
##### 09/10/23
![](unreal-1.png)
we interrupt this program to bring you: learning about blueprints/visual scripting in Unreal (it is not fun)
![](unreal-2.png)
If these seem hard to read or understand at this scale, then I have successfully taught you what visual scripting is like with a processing disorder (update: they're in better resolution now, but I BET it still looks like nonsense)

###### Continued
![](20231009-cspjar.png)
![](20231009-gdjar.png)
Testing an idea for a health HUD. "so you have a health system?" ......
![](20231009-gdhp.png)
In-game view (ft. some glitching tiles and an old second character sprite I tested after making scrimblo but eventually gave up on)
### New background and sprites
##### 03/05/24
![](shrimplebg.png)
![](shrimplewindowtestMIRR.png)
Using what I learned from the previous attempt to make a new background
![](20240503-godot-idk.png)
Along with some simple floor tiles :D ![](floortest2.png)
### Creating a pick-up/item and health system
##### 07/05/24
![](20240507-cakebad.png)
Attempting to add the cake as an item (I did it very wrong)
##### 14/05/24
![](20240514-cakeballs.png)
I did it right this time :) still no health system though. Also that little purple thing is a moving platform but i have NO IDEA when that became a thing
##### ??/??/24
The health bar actually works now. I forgot when this happened. I woke up and it worked. Also, eating the cake now heals 1HP and the orbs deal 1HP damage.

### Level design
##### 15/07/24
![](20240715-135443-leveltemp.png)
Started animating some assets like the orb and cake (they wiggle now), and added a rough layout of the actual demo level

### It's really that shrimple (more sprites and mechanics)
##### 19/07/24
![](20240719_210149-shrimpassets.png)
Making some actual platforms for the platformer and some funny shrimp clouds for the background

![](20240719_214409_signalsyay.png)
I FINALLY had to figure out what signals do in between these two things and suddenly things started making sense. I gained a brain cell!!!

![](pillars.png)
one of the platform thingies is a tileset and the other is a table/elevator that activates when you grab the cake on it :3

### Drawing the rest of the owl
##### 19/07/24-20/07/24
**Added:**
![](20240719_232555_orbsyay.png)
- Proper paths and patterns for the Danger Orbs

![](20240719_232611_shrimpforms.png)
- Groups of moving platforms that rotate and look silly


![](20240719_232618_keydoor.png)
-  A key ![](shrimpkey.png) item that opens doors

![](20240719_214516_coinheart.png)
- COINS

![](20240719_214436_NEWUI.png)
- Styling for the health bar and displays for coins/keys

![](20240719_234705oldwinscreen.png)
- A really, really ugly win screen, in which I LIED BECAUSE I HAD TIME FOR MORE STUFF

**The stuff in question:**

![](20240719_234325shrimptalk.png)
- One (1) shrimp guy (his name is Shrimpton) and his poor little son, plus little animated dialogue labels so they can yap at you

![](20240719_234103_NEWWIN.png)
- A unique ending scene that is being used as a slightly better win screen (I grew from my past mistakes) ft. Shrimpton's son, Krillip (he wasn't sick. he just didn't want to go to school)