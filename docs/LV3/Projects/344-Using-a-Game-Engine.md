### Final Product
insert video here i l;azyyyyyyyyy
### Software and assets used

| **Game engine:**        | Godot 4.3                                                                                             |
| ----------------------- | ----------------------------------------------------------------------------------------------------- |
| **Sprites & tilemaps:** | Sproutlands                                                                                           |
| **Fonts:**              | Sproutlands, ZX Palm                                                                                  |
| **Music:**              | later                                                                                                 |
| **Reference:**          | [Youtube](https://www.youtube.com/watch?v=it0lsREGdmc&lisYoutubet=PLWTXKdBN8RZe3ytf6qdR4g1JRy0j-93v9) |

### Self Evaluation
***What is the project you have been working on this term?***
> A game mechanic for a farming game

***Do you think you successfully did this?***
> mayb

***What parts of your project do you like?***
> i dont

***What could you improve on, and what part would you do differently next time?***
> i wouydl go to sleep instead

***What part of the work did you find the most difficult and why?***
> yes

### Devlog
##### 18/11/24
Starting work on a farming game with a tutorial. There was a project before this but the tutorial was a full major version behind making it super slow and I didn't like how it worked so I did this one instead. *(maybe add link to the old one idfk)*

Created a game tilemap using the Sproutlands assets and made a test scene to... test it in, and started learning how to use terrains for things like paths and grass, so they can be dynamically added/"drawn" into the scene which will speed up and simplify level creation somewhat.

![](24-11-18_2%201.png)
/// caption
Tilemap test scene
///

![](2024-11-18%2014-28-30%201.png)
/// caption
Terrain testing for paths
///


##### 19/11/24
Created a controllable player character with animations for directional movement and player actions like tilling land or watering crops.
Made a state machine and a class for node states, then used that class to create 5 different states for the player: `idle_state` and `walk_state` as the main 2, then `jopping_state` (chopping), `watering_state` and `tilling_state` for each action. I mixed up some animation directions so the player was moonwalking for a bit lol. Currently only walking and idling have transitions into each other as I haven't added tools yet and the idle animation doesn't play the correct directional animation.


![[2024-11-19 11-58-22.png]]
/// caption
Adding animation frames using the Sproutlands assets
///


??? example "Player movement code from `walk_state`"
        func _on_physics_process(_delta : float) -> void:
            var direction: Vector2 = GameInputEvents.movement_input()
            
            if direction == Vector2.UP:
                animated_sprite_2d.play("walk_back")
            elif direction == Vector2.RIGHT:
                animated_sprite_2d.play("walk_right")
            elif direction == Vector2.DOWN:
                animated_sprite_2d.play("walk_front")
            elif direction == Vector2.LEFT:
                animated_sprite_2d.play("walk_left")
            
            if direction != Vector2.ZERO:
                player.player_direction = direction
            
            player.velocity = direction * speed
            player.move_and_slide()

##### 20/11/24
Found the cause of the animation bug. The `idle_state` is supposed to get the player's direction from `walk_state` and act according to that, but I had a line of code that let it set the player's direction during their idle state, which would obviously be none/null as no actions are being pressed in idle, so it always defaulted to the front-facing animation rather than following the player's actual last direction.
 
![](2024-11-20%2011-18-57.png)
/// caption
Staring really hard at `idle_state` for 2000000 hours ASMR to figure out what I messed up
///

In preparation of implementing the different actions, I added a global script `data_types` to hold different kinds of game data (useful for things like weapons/tools) and let the player script use it to set their current tool. There's no way to change tools yet, though.

??? example "Tools framework"
	In `data_types`:
	
			class_name DataTypes
			
			enum Tools {
				None,
				AxeWood,
				TillGround,
				WaterCrops
			}
	In `player`:
	
			@export var current_tool: DataTypes.Tools = DataTypes.Tools.None


Further work on the state machine; now the player can transition to chopping, watering or tilling with  ++"Left Click"++ . I deviated from the tutorial somewhat and added transitions to the `walk_state` so that you can click while moving to stop and transition into using an action, because I prefer that than having to make sure you're idle before acting; it'll be less annoying for when you're running around watering crops etc.

I had to deal with a bug where the actions would never actually transition back so once you jopped you couldn't stop, as well as the animation bug from yesterday. I didn't take proper notes for some reason so I forgot what the problem was with the first one, but I have a suspicion I forgot this bit of code in the action states:
```
func _on_next_transitions() -> void:
	if !animated_sprite_2d.is_playing():
		transition.emit("Idle")
```


??? example "How the state machine scripts function after being fixed"
	`walk_state` script takes the direction of the player from their movement and tells it to a variable in the `player` script
	
			func _on_physics_process(_delta : float) -> void:
			
				var direction: Vector2 = GameInputEvents.movement_input()
				
						if direction != Vector2.ZERO:
							player.player_direction = direction
	it also checks for when there's no movement input and correctly transitions to `idle_state`
	
			func _on_next_transitions() -> void:
			if !GameInputEvents.is_movement_input():
				transition.emit("Idle")
	
	---
	
	`idle_state` script reads the direction variable and plays the corresponding animation
	
			func _on_physics_process(_delta : float) -> void:
				if player.player_direction == Vector2.UP:
					animated_sprite_2d.play("idle_back")
				elif player.player_direction == Vector2.DOWN:
					animated_sprite_2d.play("idle_front")
				elif player.player_direction == Vector2.LEFT:
					animated_sprite_2d.play("idle_left")
				elif player.player_direction == Vector2.RIGHT:
					animated_sprite_2d.play("idle_right")
				else:
					animated_sprite_2d.play("idle_front")
	and then... you're not gonna believe this. it checks for when there IS movement input and transitions to `walk_state`
	
			func _on_next_transitions() -> void:
				GameInputEvents.movement_input()
				
				if GameInputEvents.is_movement_input():
					transition.emit("Walk")
	
	---
	
	The `_on_next_transitions` function from the template is also how the various actions are handled currently.
	
				if  Input.is_action_just_pressed("hit") && player.current_tool == DataTypes.Tools.AxeWood:
					transition.emit("Jopping")
				elif Input.is_action_just_pressed("hit") && player.current_tool == DataTypes.Tools.TillGround:
					transition.emit("Tilling")
				elif Input.is_action_just_pressed("hit") && player.current_tool == DataTypes.Tools.WaterCrops:
					transition.emit("Watering")

##### 25/11/24
Back to tilemap stuff. Creating multiple different houses, each as their own scene and using a house-specific tileset, before adding them to the main game tileset using scene collections so they can be instantiated and deleted as if they were one tile. 
![](2024-11-25%2011-58-53.png) ![](2024-11-25%2012-08-01.png)
/// caption
Adding the new house tileset, with layers for the floor, walls and furniture plus collision for objects and walls
///

![](2024-11-25%2012-14-48.png)![](2024-11-25%2012-18-08.png)
/// caption
HELL YEAAAAAAAAAAAA
///



Additionally created doors using the same method of using a scene collection to place the door scene in the tilemap, because they need a lot more functionality than a regular tile. They use an animated sprite, with only one animation, because for a smooth opening/closing transition even if the player quickly passes by before it fully opens, it'll be better to just reverse the animation rather than start a new one.

To make the doors actually functional, they use a component; reusable scenes that contain a core function (being collected, growing, taking damage etc) and can essentially just be tossed onto a scene to give it that functionality. The component they use is an `interactable_component` that will allow the door to detect when it's being interacted with (when the player approaches).

??? example "Interactable component script (area2d)"
			class_name InteractableComponent
			extends Area2D
			
			signal interactable_activated
			signal interactable_deactivated
			
			
			
			@warning_ignore("unused_parameter")
			func _on_body_entered(body: Node2D) -> void:
				interactable_activated.emit()
			
			@warning_ignore("unused_parameter")
			func _on_body_exited(body: Node2D) -> void:
				interactable_deactivated.emit()

??? example "Door script"
			extends StaticBody2D
			@onready var animated_sprite_2d: AnimatedSprite2D = $AnimatedSprite2D
			@onready var collision_shape_2d: CollisionShape2D = $CollisionShape2D
			@onready var interactable_component: InteractableComponent = $InteractableComponent
			
			func _ready() -> void:
				interactable_component.interactable_activated.connect(on_interactable_activated)
				interactable_component.interactable_deactivated.connect(on_interactable_deactivated)
				collision_shape_2d.set_deferred("disabled", false)
			
			func on_interactable_activated() -> void:
				animated_sprite_2d.play("open_door")
				collision_shape_2d.set_deferred("disabled", true)
			
			
			func on_interactable_deactivated() -> void:
				animated_sprite_2d.play("close_door")
				collision_shape_2d.set_deferred("disabled", false)
				print("door deactivated")



##### 02/12/24
Added trees and a chopping function. Trees are made as their own separate scenes, then added to the main game tileset using scene collections. This means they can have their own scripts and functions despite having the ease of editing that comes with being a tile, as well as "counting" as only one tile regardless of their size, so there's a little more freedom of placement.

The way chopping works is that the the player's chopping state, `jopping_state`, now enables and moves a collision shape that deals damage to them on contact, and when its HP hits 0 it disappears and replaces itself with a collectable log. There's no inventory though, so they just kind of pop out of existence.

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
Starting to add a save system, using P as a shortcut to save and adding various components that will allow the game save certain parts of the scene, such as the player-modified tiles and objects such as farmland and crops. It's a lot of scripting basically.

There's also a bug where you can plant multiple crops on one tile, and I tried to fix it but it hasn't worked. that's for later I guess.

##### 20/01/25
Apparently I forgot to add collision keeping the player from walking off the island so I did that lol. Making the actual game environment now, in a way that would allow for multiple different "levels" though I don't use it because the only buildings and areas in the game are fine being
contained within one level. 

The chickens are stupid again. I don't know why.
Y sorting doesn't work on the level I also dk why

Started making a menu screen, all I got to do was add a button, theming isn't done yet

##### 21/01/25
Finished the title/pause menu you can bring up using Esc, with options to Start, Save and Quit which all technically work (the functions need bug fixing and pausing doesn't pause yet) we will get back t o that one. The background is basically a level, but frozen using process mode disabled thing yayyyyyyy.
I didn't decorate the background yet but that's just for aesthetics .

##### 22/01/25
Decorated the menu background. im sleepy

##### 27/01/25
Pausing functionality added using get_tree().paused and setting the menu's process mode to always, so the entire game will freeze except for the pause menu. Way easier than pausing just the level and potentially missing things. Saving and loading now properly works on main-game scenes, and I fixed logs being set to add 0 of themselves when collected. The game is basically done as far as the assignment is concerned though I am putting off audio like my life depends on it because my headphones don't reach that far lmaoooo
