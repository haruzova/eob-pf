### Final Product
insert video here i l;azyyyyyyyyy

| **Game engine:**               | Godot 4.3            |
| ------------------------------ | -------------------- |
| **Sprites, tilemaps & fonts:** | Sproutlands, ZX Palm |
| **Music:**                     | add                  |
| **Guide:**                     | link                 |
### Self Evaluation
***What is the project you have been working on this term?***
- d
***Do you think you successfully did this?***
- d
***What parts of your project do you like?***
- s
***What could you improve on, and what part would you do differently next time?***
- s
***What part of the work did you find the most difficult and why?***
- s

### Devlog
##### 18/11/24
Starting work on a farming game with a tutorial. There was a project before this but the tutorial was a full major version behind making it super slow and I didn't like how it worked so I did this one instead. *(maybe add link to the old one idfk)*
##### 19/11/24
Creating a controllable player character with an animation tree for movement, and starting work on a state machine for different actions like tilling land or watering crops.
Ran into an issue where animations wouldn't play and some of the movement animations were facing the wrong way, fixed it by reordering the keys(????? are they keys?) so they encouraged the correct animation for the direction. TLDR I mixed up the Y axis lol
##### 20/11/24
Further work on the state machine, now the player can transition to chopping, watering or tilling. I deviated from the tutorial and made it so you can click while moving to stop and transition into using an action, because I prefer that than having to manually stop just to act. Apparently I ran into some bugs but fool that I am I did not even bother to explain what they were so I no longer know what the bugs are. Beautiful <3

##### 25/11/24
More tilemap stuff. Creating multiple different houses, each in their own scene and using a house-specific tilemap, before adding them to the game tilemap using scene collections so they can be instantiated and deleted as if they were one tile. I added functional doors using the same method (separate door scene, then instantiating it in the houses' tilemap) and added a script so they automatically open and close in response to the player.

##### 26/11/24
ok i lied im websiteing i think

##### 27/11/24
still websiteing

##### 02/12/24
Adding trees and a chopping function. Trees are using scene collections so they can be placed on the tilemap while being functional. The player's "chopping" state enables a collision shape that deals damage to them on contact, and when its HP hits 0 it disappears and replaces itself with a collectable log. There's no inventory though, so they just kind of pop out of existence.

##### 03/12/24
Adding a larger tree with more HP, as well as destructible rocks with similar code that drop stones instead, and adding a shake animation to each of them using shaders. Now when the player hits a tree or rock, they'll shake in response to it. There was a little issue where hitting one of an object would shake all of the others, but that was fixed by ticking "local to scene" on the shaders.

##### 04/12/24
Adding Y-sorting to the world and tilemaps. I lost the screenshots but it's just a bunch of walking behind/in front of trees anyway. Starting work on chicken NPCs with their own animations, navigation and state machine. They're a bit stupid now, running into walls, endlessly walking (transition into idle state isn't working yet), bumping into each other, etc and all of them start walking at the same time on start so they look goofy. 
##### 09/12/24
Fixed the chickens. They now walk around obstacles, avoid each other, properly cycle between walking and idling and aren't all in sync anymore so it looks more natural. Using the same framework I added cow NPCs that use a different navigation layer, as well as a terrain for fences that the navigation shapes automatically exclude so nobody tries to path into a fence or anything.

##### 10/12/24
Finishing the pathfinding for cows, and moving on to the game UI. There's a toolbar that lets the player change tools, and an inventory bar that displays all the items and their count. To streamline the process when adding new elements later, as well as for consistency, all UI elements are styled by the same theme, with different types for the different... types of element... That makes sense it just sounds weird okay

Also added new collectibles like milk, eggs, wheat, tomatoes, and seeds using the same collectable component I made the rocks and logs from :)

#####  11/12/24
Finishing the inventory panel by adding custom fonts and item counters (also adding counters for seeds in the toolbar because it makes sense), then finally making collectables do something with the use of a global script that manages the inventory and modifying the collectable component so it adds items.

Adding a time mechanism along with new panels showing the current day and time, with some speed options so you can speed up the day. There was an issue where the time didn't update because I forgot to actually use the recalculate_time() function so it... never recalculated the time. (Sapient lifeform btw)
 
#####  16/12/24
Completed the day-night cycle with a component to control the initial day and time that also adds a filter to the game screen with a custom gradient that changes over the course of a day, so as the day progresses the game appears to have a sunset, sunrise and night-time.

Added some crops and gave them various particle effects for when they're freshly watered, wet, and mature. They use the hurt component from the trees and rocks to determine when they're being watered, and a new growth component controls their growth (WOAH NO WAY).

They're a bit buggy, and I don't like how the tutorial handles crops. As it stands, when they're watered once they don't ever have to be watered again, but the wet particles disappear after a while so you can't tell which crops are watered. They will "harvest" themselves automatically after a day, which leaves the player with not much to actually... do, and gives the impression the goods are just sitting on the ground being nibbled on by bugs all night. Additionally, the final harvested stage, despite looking like an item, actually isn't one and is uncollectable. They also look kinda boring when they just pop into the next growth stage, and the way the growth cycle is done means that even with a growth length of 5 days for the 5 visible stages, the sprites don't update predictably, so two crops that look the same may be harvested on different days. There's a lot to fix, but I'll just move on for now...

#####  18/12/24
Finished the crops part of the tutorial, adding some tomatoes, then moving back to Y-sorting for a bit to add different origin points for the various tiles and objects like crops, so they sort from a point that looks believable for their sprite.

Added a new terrain for tilled dirt and started work on a script and component that will let the player add farmland by using the hoe on nearby grass tiles.

#####  06/01/25
Fixed an issue where tomatoes specifically would be Eternally Wet because it looked weird (even if I dislike not knowing which crops are watered...), turns out I forgot to connect a signal so they ignored the day ticking over. Also made it so that when a plant grows it emits the "maturity" particles for a short while so it's less boring. There's no internet right now, so I have to get silly with it. Tomorrow I will add subway surfers and family guy funny moments to the UI o7

##### 07/01/25
Crops growth rework. They now need to be watered every day, only progressing one stage a day, and once they're mature they stop growing and wait for the player. Added a new component specifically for harvesting crops (the hurt component is already being used for watering, so its collision shape will only listen for the watering can, and I'd rather use a different signal for harvesting for simplicity), so now the player can use the hoe to harvest crops. Added a damage component and a new harvesting function, so that prior to maturity the player can dig up crops and get their seeds back, with some digging dirt particles to go with it.

``` harvesting-function
func on_harvest(hit_damage: int) -> void:
	if growth_state == DataTypes.GrowthStates.Mature:
		on_crop_harvesting()
	else:
		damage_component.apply_damage(hit_damage)
		await get_tree().create_timer(0.4).timeout
		digging_particles.emitting = true
```

Adding a seeds counter to the toolbar, and making it so that when a crop is harvested the player gets some seeds back automatically, so you don't slowly run out of seeds. Also tweaking the inventory manager global and collectable component so that items can give multiple of themselves, meaning a mature crop now gives you back 2 seeds instead of 1.

08/01/25
Due to the way the inputs were handled in the tutorial script, clicking on any UI element would trigger an action (chopping, tilling etc), and I don't like that. If you're holding the hoe in front of a crop and go to switch to the watering can, the player will damage the crop beforehand (they have 2 HP and each hit only deals 1 damage, but still) which is annoying. To fix this I moved the clicking actions into \_unhandled\_input(), giving them a low priority. Now, because the UI handles the inputs first when you click on it, the game world underneath it pretends not to see it and everyone lives happily ever after. There was a bit of a problem where the player would "double act", due to how I implemented the ability to transition into actions while walking (both scripts would detect the input as both scripts had the action transitions in them, and I just moved them out of the state-specific functions), and I fixed it by just adding a variable to idle_state and walk_state that changed to true/false depending on whether the player is actually in their respective states, then tossing it onto the conditions for each action so that only one of the two main states can transition to an action at any time. That sounds convoluted but look it's literally just...

```walk_state
var state: bool

func _unhandled_input(event: InputEvent) -> void:
	if  Input.is_action_just_pressed("hit") && player.current_tool == DataTypes.Tools.AxeWood && state == true:
		transition.emit("Jopping")
	elif Input.is_action_just_pressed("hit") && player.current_tool == DataTypes.Tools.TillGround && state == true:
		transition.emit("Tilling")
	elif Input.is_action_just_pressed("hit") && player.current_tool == DataTypes.Tools.WaterCrops && state == true:
		transition.emit("Watering")
```

Continued work on the tilling script but I didn't get far lol

#####  13/01/25
(missing documentation, screenshots are from the final thing sowwy)
Also continued work on tilling, finishing the scripts for the field cursor component and crops cursor component; they are *meant* to let you place tilled dirt and crops, but it's not actually placing them for some reason.

#####  14/01/25
Found what was causing the issue, it was that the tilled soil layer didn't have a tileset set... Better that than a complicated bug I guess. Added some more tiles and fixed the bitmask for the tilled soil, since it turns out the way they set it in the tutorial doesn't allow for certain shapes like a T shape... and I messed up the bitmask before. But now it's cool :) You can also un-till soil by pressing right click, and crops are only able to be planted on tilled ground so you gotta make little fields.

Also ignored the tutorial for a bit to make tools de-selectable, I just don't like that once you select one it's all over. And being able to not have tools in your hands means maybe... one day... you can pet the cows and chickens... surely......... I also made the seeds actually do something, so now you only plant crops if you have enough, and each time you plant a crop, the respective seeds go down by 1. That was actually surprisingly hard, there was a bug where if you started the game and tried to plant seeds without ever picking up or losing any it'd freak out because it was drawing from a null value. I fixed it by making the inventory manager "add" 0 to the items on start, so that if they're null they get assigned a value but if you already have some items it won't change that.

##### 15/01/25
Starting to add a save system, using P as a shortcut to save and adding some components that will save certain parts of the scene, like the player-modified tiles and objects such as farmland and crops. It's a lot of scripting basically.

There's also a bug where you can plant multiple crops on one tile, and I tried to fic it but it hasn't worked. that's for later I guess.

##### 20/01/25
##### 21/01/25
##### 22/01/25
##### 27/01/25

