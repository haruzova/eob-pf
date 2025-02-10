# 344 - Using a Game Engine
## Final Product
insert video here i l;azyyyyyyyyy
## Software and assets used

| **Game engine:**        | Godot 4.3                                                                                             |
| ----------------------- | ----------------------------------------------------------------------------------------------------- |
| **Sprites & tilemaps:** | Sproutlands                                                                                           |
| **Fonts:**              | Sproutlands, ZX Palm                                                                                  |
| **Music:**              | later                                                                                                 |
| **Reference:**          | [Youtube](https://www.youtube.com/watch?v=it0lsREGdmc&lisYoutubet=PLWTXKdBN8RZe3ytf6qdR4g1JRy0j-93v9) |

## Self Evaluation
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

## Devlog
### Test scene & Tilemap
##### 18/11/24
Starting work on a farming game with a tutorial. There was a project before this but the tutorial was a full major version behind making development super slow (also I didn't like how the mechanics worked) so I did this one instead. *(maybe add link to the old one idfk)*

Created a game tilemap using the Sproutlands assets and made a test scene to... test it in, and started learning how to use terrains for things like paths and grass, so they can be dynamically added/"drawn" into the scene which will speed up and simplify level creation somewhat.

![](24-11-18_2%201.png)
/// caption
Tilemap test scene
///

![](2024-11-18%2014-28-30%201.png)
/// caption
Terrain testing for paths
///

### Player Animations & State Machine
##### 19/11/24
Created a player scene, and added animations for walking, idling, chopping, tilling and watering in 4 directions as well as some collision.

![[2024-11-19 11-58-22.png]]
/// caption
Adding animation frames using the Sproutlands assets
///


Made a state machine and a class for node states, then used that class to create 5 different states for the player for each action. Currently only walking and idling have transitions into each other, as I haven't added tools yet, and the idle animation keeps defaulting to the front facing one regardless of which way the player should be facing which is weird.

	``` gdscript title="walk_state.gd"
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
	``` 

##### 20/11/24
Found the cause of the animation bug! The `idle_state` is supposed to get the player's direction from `walk_state` and act according to that, but I had a line of code that let it set the player's direction during their idle state, which would obviously be none/null as no actions are being pressed in idle, so it always defaulted to the front-facing animation rather than following the player's actual last direction.
 
![](2024-11-20%2011-18-57.png)
/// caption
Staring really hard at `idle_state` for 2000000 hours ASMR to figure out what I messed up
///

In preparation of implementing the different actions, I added a global script `data_types` to hold different kinds of game data (useful for things like weapons/tools) and let the player script use it to set their current tool. There's no way to change tools yet, though.

??? example "Basic tools framework"
	``` gdscript title="data_types.gd"
	class_name DataTypes
				
	enum Tools {
		None,
		AxeWood,
		TillGround,
		WaterCrops
	}
	```

	``` gdscript title="player.gd"
	@export var current_tool: DataTypes.Tools = DataTypes.Tools.None

	```


Further work on the state machine; now the player can transition to chopping, watering or tilling with  ++"Left Click"++ . I deviated from the tutorial somewhat and added transitions to the `walk_state` so that you can click while moving to stop and transition into using an action, because I prefer that than having to make sure you're idle before acting; it'll be less annoying for when you're running around watering crops etc.

I had to deal with a bug where the actions would never actually transition back so once you jopped you couldn't stop, as well as the animation bug from yesterday. I didn't take proper notes for some reason so I forgot what the problem was with the first one, but I have a suspicion I forgot this bit of code in the action states:
``` gdscript title="example_action_state.gd"
func _on_next_transitions() -> void:
	if !animated_sprite_2d.is_playing():
		transition.emit("Idle")
```


??? example "How the state machine scripts function after being fixed"
	``` gdscript title="walk_state.gd" hl_lines="4 7 11 12"
	# gets the direction of the player as they walk and sends the result to a variable in player.gd
	func _on_physics_process(_delta : float) -> void:
				
		var direction: Vector2 = GameInputEvents.movement_input()
					
				if direction != Vector2.ZERO:
					player.player_direction = direction

	# checks for when there's no movement input and transitions to idle
	func _on_next_transitions() -> void:
		if !GameInputEvents.is_movement_input():
			transition.emit("Idle")
	```

	``` gdscript title="idle_state.gd"
	# reads the direction variable from player.gd and plays the corresponding animation
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

	# checks for when there IS movement input and transitions to walking
	func _on_next_transitions() -> void:
		GameInputEvents.movement_input()
					
		if GameInputEvents.is_movement_input():
			transition.emit("Walk")
	```
	The `_on_next_transitions` function from the template is also how the various actions are handled currently.
	``` gdscript title="idle_state.gd & walk_state.gd"
	func _on_next_transitions() -> void:
		if  Input.is_action_just_pressed("hit") && player.current_tool == DataTypes.Tools.AxeWood:
			transition.emit("Jopping")
		elif Input.is_action_just_pressed("hit") && player.current_tool == DataTypes.Tools.TillGround:
			transition.emit("Tilling")
		elif Input.is_action_just_pressed("hit") && player.current_tool == DataTypes.Tools.WaterCrops:
			transition.emit("Watering")
	```
### Houses & Destructible Objects
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


Created doors using the same method of making a separate scene and then adding it to the tileset with scene collections, because they'll need a lot more functionality than a regular tile. They use an animated sprite, with only one animation, because for a smooth opening/closing transition even if the player quickly passes by before it fully opens, it's better to just reverse the animation rather than play a new one.

To make the doors actually functional, they use a component; essentially reusable scenes with scripts that contain a basic function (being collected, growing, taking damage etc) and can more or less just be tossed onto a scene to give it that functionality, then specialised and extended as needed. The component the doors use is an `interactable_component` that will allow the door to detect when it's being interacted with; in this case, when the player approaches.

??? example "Door scripts"
	``` gdscript title="interactable_component.gd"
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
	```

	``` gdscript title="door.gd"
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
	```

(add gif/video of going to tha door xd)

##### 02/12/24
Added some small trees. Since they need to be chopped down and drop items, they're being added to the tileset using scene collections as well. Created a few new components; `hit_component` and `hurt_component` for player and environment hitboxes, `damage_component` for taking/inflicting damage and `collectable_component` for collectable items. The trees each have their own HP, and the player's actions will now move a hitbox in front of them which can deal damage to objects whose type matches the player's current tool, disappearing at 0 and dropping an item that can be picked up. There is no inventory system yet though, so they just kinda disappear.

The `hit_component` in the player scene is given its area by a small collision shape just for tools that is disabled by default, and each of the action states have been modified slightly to enable this hitbox and move it to the direction of the player. The component script gets the player's current tool, and how much damage they're supposed to deal, each as variables.

(player doing the hit component thing gif  idk)


The trees likewise have their `hurt_component` with its own collision shape and a specified tool to look out for; when the `hit_component` enters the `hurt_component` with the matching tool selected, it emits a signal containing how much damage the player deals. This signal gets picked up by the `damage_component` on the trees which handles their HP, and the component decreases their current HP accordingly. Once their HP hits 0, `damage_component` emits a different signal, and the `tree` script recieves that signal and places a log scene before politely disappearing.

(tree jopping down show)

The log is simple, it has a `collectable_component` with a shape that looks out for the player, and once the player's main hitbox enters that shape, the component kicks the log out of the scene.

(log magic show)


??? example "Component scripts"
	``` gdscript title="hit_component.gd"
	class_name HitComponent
	extends Area2D

	@export var current_tool : DataTypes.Tools = DataTypes.Tools.None
	@export var hit_damage : int = 1
	```

	``` gdscript title="hurt_component.gd"
	class_name HurtComponent
	extends Area2D

	@export var tool : DataTypes.Tools = DataTypes.Tools.None

	signal hurt

	func _on_area_entered(area: Area2D) -> void:
		var hit_component = area as HitComponent
		
		if tool == hit_component.current_tool:
			hurt.emit(hit_component.hit_damage)
	```

	``` gdscript title="damage_component.gd"
	class_name DamageComponent
	extends Node2D


	@export var max_damage = 1
	@export var current_damage = 0

	signal max_damage_reached

	func apply_damage(damage: int) -> void:
		current_damage = clamp(current_damage + damage, 0, max_damage)
		
		if current_damage == max_damage:
			max_damage_reached.emit()
	```
	``` gdscript title="collectable_component.gd"
	class_name CollectableComponent
	extends Area2D

	@export var collectable_name: String
	@export var collectable_count: int

	func _on_body_entered(body: Node2D) -> void:
		if body is Player:
			InventoryManager.add_collectable(collectable_name, collectable_count)
			print("Collected:", collectable_name)
			get_parent().queue_free()
	```


##### 03/12/24
Added a larger tree with some more HP, as well as a rock that uses all the same components but drop stones instead.
(rock and tree pic. the destructible object brothers)


Added an animation to all three destructible objects using shaders. Now when the player hits a tree or rock, they'll shake in response to it. There was a little issue where hitting one of an object would shake all of the others, but just ticking the "local to scene" box on the shaders solves that.
(wiggle show)


``` gdscript title="shake.gdshader"
shader_type canvas_item;

uniform float shake_intensity = 0.0;
uniform float shake_speed = 20.0;

void vertex() {
	// Called for every vertex the material is visible on.
	vec2 shake = vec2(0,0);

	if(VERTEX.y < 0.0) {
		shake.x = sin(TIME * shake_speed + VERTEX.y) * shake_intensity;

	}
	VERTEX.xy += shake;
}
```

### NPCs and Pathfinding

##### 04/12/24
Adding Y-sorting to the world and tilemaps. It's just a bunch of ticking Y sort boxes lmao. 
(show tree)

Starting work on chicken NPCs with their own animations and state machine similar to the player, with a NavigationAgent2D node that will be used for pathfinding. A NavigationRegion2D in the scene they're in, and on the same navigation layer as them, determines where the chickens are able to path to (defined by a NavigationPolygon), and their state machine should make them cycle between walking around their NavigationPolygon and idling.

Unfortunately, the chickens are currently a bit stupid. They're running into walls, endlessly walking (transition into idle state isn't working yet), bumping into each other, etc and all of them start walking at the same time on game start so they look goofy. 


##### 09/12/24
Fixed the chickens. They now walk around obstacles, properly avoid each other, as their path postprocessing mode is now set to Edgecentered, and the script has been checked over so now they properly cycle between walking and idling, and tweaked so they aren't all in sync anymore so it looks more natural. Using the same framework as the chickens, I added cow NPCs that use a different navigation layer, as well as a terrain for fences that the navigation shapes automatically exclude so nobody tries to path into a fence or anything. This was achieved by adding the fences' tilemap layer to a global group, and setting the NavigationPolygon base (which both NPCs use) to look for tiles in that group and remove their area from the polygon so I don't have to manually do so with every little change in fence layout.

??? example "Final NPC state machine & Navigation"
	``` gdscript title="chicken.gd"
	extends NonPlayableCharacter
	# randomises each chicken's number of walk cycles before they start idling
	func _ready() -> void:
		walk_cycles = randi_range(min_walk_cycle, max_walk_cycle)
	```

	``` gdscript title="npc_states/idle_state.gd"
	extends NodeState

	@export var character: CharacterBody2D
	@export var animated_sprite_2d: AnimatedSprite2D
	@export var idle_state_time_interval: float = 1

	@onready var idle_state_timer: Timer = Timer.new()

	var idle_state_timeout: bool = false

	# randomises the time each chicken will spend in their idle state and connects the relevant signals
	func _ready() -> void:
		idle_state_timer.wait_time = idle_state_time_interval + randf_range(1, 4)
		idle_state_timer.timeout.connect(on_idle_state_timeout)
		add_child(idle_state_timer)

	func _on_process(_delta : float) -> void:
		pass


	func _on_physics_process(_delta : float) -> void:
		pass

	# transitions to walking if the timer has ended
	func _on_next_transitions() -> void:
		if idle_state_timeout:
			transition.emit("Walk")


	func _on_enter() -> void:
		animated_sprite_2d.play("Idle")
		
		idle_state_timeout = false
		idle_state_timer.start()

	func _on_exit() -> void:
		animated_sprite_2d.stop()
		idle_state_timer.stop()

	# changes a variable when the idle timer is finished
	func on_idle_state_timeout() -> void:
		idle_state_timeout = true
	```

	``` gdscript title="npc_states/walk_state.gd"
	extends NodeState

	@export var character: NonPlayableCharacter
	@export var animated_sprite_2d: AnimatedSprite2D
	@export var navigation_agent_2d: NavigationAgent2D
	@export var min_speed: float = 10.0
	@export var max_speed: float = 20.0

	var speed: float

	# sets up any signal-function connections and moves to character setup
	func _ready() -> void:
		navigation_agent_2d.velocity_computed.connect(on_safe_velocity_computed)
		
		call_deferred("character_setup")

	func character_setup() -> void:
		await get_tree().physics_frame
		
		set_movement_target()

	# sets the movement target to a reasonable location and sets the character's speed to be slightly randomised
	func set_movement_target() -> void:
		var target_position: Vector2 = NavigationServer2D.map_get_random_point(navigation_agent_2d.get_navigation_map(), navigation_agent_2d.navigation_layers, false)
		navigation_agent_2d.target_position = target_position
		speed = randf_range(min_speed, max_speed)
			

	func _on_process(_delta : float) -> void:
		pass

	# counts when the character finishes a walk cycle and sets a new movement target
	func _on_physics_process(_delta : float) -> void:
		if navigation_agent_2d.is_navigation_finished():
			character.current_walk_cycle += 1
			set_movement_target()
			return

		#flips the character sprite based on where they're heading relative to their location
		var target_position: Vector2 = navigation_agent_2d.get_next_path_position()
		var target_direction: Vector2 = character.global_position.direction_to(target_position)
		animated_sprite_2d.flip_h = target_direction.x < 0
		
		# handles character movement
		var velocity: Vector2 = target_direction * speed
		
		if navigation_agent_2d.avoidance_enabled:
			navigation_agent_2d.velocity = velocity
		else:
			character.velocity = target_direction * speed
			character.move_and_slide()
		

	func on_safe_velocity_computed(safe_velocity: Vector2) -> void:
		character.velocity = safe_velocity
		character.move_and_slide()

	# transitions to idle when the number of walk cycles is reached
	func _on_next_transitions() -> void:
		if character.current_walk_cycle == character.walk_cycles:
			character.velocity = Vector2.ZERO
			transition.emit("Idle")
			

	# resets the current walk cycles to 0 when re-entering the state
	func _on_enter() -> void:
		animated_sprite_2d.play("Walk")
		character.current_walk_cycle = 0


	func _on_exit() -> void:
		animated_sprite_2d.stop()
	```



### UI for Game Mechanics

##### 10/12/24
Tweaked the cows a little to make them a little less active than chickens, now moving on to the game UI. Added a toolbar that lets the player change tools, and an inventory bar that displays all the items and their count. The UI elements are styled by a game theme, with different custom "types" for the various types of elements, so all the tool buttons can be edited at once or have specific overrides.

Also added new collectibles like milk, eggs, wheat, tomatoes, and seeds using the same collectable component I made the rocks and logs from :)
(show collectables)

#####  11/12/24
Finishing the inventory panel by adding custom fonts and item counters (also adding counters for seeds in the toolbar because it makes sense), then finally making collectables do something with the use of a global script that manages the inventory and modifying the collectable component so it adds items. I'll go over the scripts later, as they get a small rework pretty soon.

Adding a time mechanism along with new panels showing the current day and time, with some speed options so you can speed up the day. There was an issue where the time didn't update because I forgot to actually use the recalculate_time() function so it... never recalculated the time. Mindblowing.

??? example "Time & Date scripts"
	``` gdscript title="day_and_night_cycle_manager.gd"
	extends Node

	# maths. if i explain this further i think i might throw up sorry
	const MINUTES_PER_DAY: int = 24 * 60
	const MINUTES_PER_HOUR: int = 60
	const GAME_MINUTE_DURATION: float = TAU / MINUTES_PER_DAY

	var game_speed: float = 5.0

	var initial_day: int = 1
	var initial_hour: int = 12
	var initial_minute: int = 30

	var time: float = 0.0
	var current_minute: int = -1
	var current_day: int = 0

	signal game_time(time:float)
	signal time_tick(day: int, hour: int, minute: int)
	signal time_tick_day(day: int)

	func _ready() -> void:
		set_initial_time()
		

	func _process(delta: float) -> void:
		time += delta * game_speed * GAME_MINUTE_DURATION
		game_time.emit(time)
		
		recalculate_time()

	func set_initial_time() -> void:
		var initial_total_minutes = initial_day * MINUTES_PER_DAY + (initial_hour * MINUTES_PER_HOUR) + initial_minute
		
		time = initial_total_minutes * GAME_MINUTE_DURATION

	func recalculate_time() -> void:
		var total_minutes: int = int(time / GAME_MINUTE_DURATION)
		var day: int = int(total_minutes / MINUTES_PER_DAY)
		var current_day_minutes: int = total_minutes % MINUTES_PER_DAY
		var hour: int = int(current_day_minutes / MINUTES_PER_HOUR)
		var minute: int = int(current_day_minutes % MINUTES_PER_HOUR)
		
		if current_minute != minute: 
			current_minute = minute
			time_tick.emit(day, hour, minute)
		
		if current_day != day:
			current_day = day
			time_tick_day.emit(day)
	```
	``` gdscript title="day_night_panel.gd"
		extends Control

		@onready var day_label: Label = $DayPanel/MarginContainer/DayLabel
		@onready var time_label: Label = $TimePanel/MarginContainer/TimeLabel

		@export var normal_speed: int = 2
		@export var fast_speed: int = 10
		@export var light_speed: int = 300

		func _ready() -> void:
			DayAndNightCycleManager.time_tick.connect(on_time_tick)
			DayAndNightCycleManager.game_speed = normal_speed

		func on_time_tick(day: int, hour: int, minute: int) -> void:
			day_label.text = "Day " + str(day)
			time_label.text = "%02d:%02d" % [hour, minute]


		func _on_normal_spd_button_pressed() -> void:
			DayAndNightCycleManager.game_speed = normal_speed


		func _on_fast_spd_button_pressed() -> void:
			DayAndNightCycleManager.game_speed = fast_speed


		func _on_light_spd_button_pressed() -> void:
			DayAndNightCycleManager.game_speed = light_speed
	```


### Adding Farming to the Farming Sim

#####  16/12/24
Completed the day-night cycle with a component to control the initial day and time that also adds a filter to the game screen with a custom gradient that changes over the course of a day, so as the day progresses the game appears to have a sunset, sunrise and night-time.

Added some corn crops and gave them various particle effects for when they're actively being watered, wet, and mature. They use the same hurt component from the trees and rocks (listening for the watering can this time) to determine when they're being watered, and a new growth component controls their growth (WOAH NO WAY).

They're a bit buggy at present, and I don't particularly like how the tutorial handles crops. As it stands, when they're watered once they don't ever have to be watered again, but the wet particles disappear after a while so you can't tell which crops are watered. They will "harvest" themselves automatically after a day, which leaves the player with not much to actually do, and gives the impression the goods are just sitting on the ground being nibbled on by bugs all night. Additionally, the final harvested stage, despite looking like an item, actually isn't one and is uncollectable. They also look kinda boring when they just pop into the next growth stage, and the way the growth cycle is done means that even with a growth length of 5 days for the 5 visible stages, the sprites don't update predictably, so two crops that look the same may be harvested on different days. There's a lot to fix, but I'll just move on for now...

#####  18/12/24
Finished the crops part of the tutorial, adding some tomatoes that work the same as the corn, then moving back to Y-sorting for a bit to add different origin points for the various tiles and objects like crops, so they sort from a point that looks believable for their sprite.

Added a new terrain for tilled dirt and started work on a script and component that will let the player add farmland by using the hoe on nearby grass tiles.

#####  06/01/25
Fixed a visual issue where tomatoes specifically would be Eternally Wet because it looked weird (even if I dislike not knowing which crops are watered...), turns out I forgot to connect a signal so they ignored the day ticking over. Also made it so that when a plant grows it emits the "maturity" particles for a short while so it's less boring. There's no internet right now, so I have to get silly with it. Tomorrow I will add subway surfers and family guy funny moments to the UI o7

(show particles)

##### 07/01/25
JUMPSCARE!!! It's a crops rework because I can't take it anymore. Crops now need to be watered every day, progressing one stage a day with each stage being directly correlated with a new sprite, and once they're mature they stop growing and wait to be harvested. On that note, I also added a new component specifically for harvesting crops (the hurt component is already being used for watering, and I'd rather not mess with the existing function too much and hurt my brain, so a new component it is), so now the player can use the hoe to harvest crops when they're mature. Added a damage component and a new harvesting function as well, so that prior to crop maturity, the player can dig up crops and get their seeds back, with some cute lil digging dirt particles to go with it.

??? example "Growth cycle component and harvest component"
	``` gdscript title="harvest_component.gd"
	class_name HarvestComponent
	extends Area2D

	@export var tool : DataTypes.Tools = DataTypes.Tools.None

	signal harvest

	func _on_area_entered(area: Area2D) -> void:
		var hit_component = area as HitComponent
		
		if tool == hit_component.current_tool:
			harvest.emit(hit_component.hit_damage)
	```

	``` gdscript title="growth_cycle_component.gd"
	class_name GrowthCycleComponent
	extends Node

	@export var current_growth_state: DataTypes.GrowthStates = DataTypes.GrowthStates.Sprout
	@export_range(1,365) var days_until_harvest: int = 7

	signal crop_maturity
	signal crop_harvesting

	var is_watered: bool
	var growth_days: int = 1

	func _ready() -> void:
		DayAndNightCycleManager.time_tick_day.connect(on_time_tick_day)

	func on_time_tick_day(day: int) -> void:
		if is_watered and current_growth_state != DataTypes.GrowthStates.Mature:
			growth_days += 1
			current_growth_state = growth_days
			print("Current growth state: ", DataTypes.GrowthStates.keys()[current_growth_state])
		
		#if current_growth_state == DataTypes.GrowthStates.Mature:
			#growth_days += 1



	func growth_states(growth_days: int):
		if current_growth_state == DataTypes.GrowthStates.Mature:
			return
		
		current_growth_state = growth_days
		print("Current growth state: ", DataTypes.GrowthStates.keys()[current_growth_state])
		
		if current_growth_state == DataTypes.GrowthStates.Mature:
			crop_maturity.emit()

	func get_current_growth_state() -> DataTypes.GrowthStates:
		return current_growth_state
	```




??? example "Actual crop script. It's in a separate admonition for a reason."
	``` gdscript title="corn.gd"
	extends Node2D

	var harvest_scene = preload("res://scenes/objects/plants/corn_harvest.tscn")
	var seeds_scene = preload("res://scenes/objects/plants/corn_seeds.tscn")

	@onready var sprite_2d: Sprite2D = $Sprite2D
	@onready var watering_particles: GPUParticles2D = $WateringParticles
	@onready var wet_particles: GPUParticles2D = $WetParticles
	@onready var growing_particles: GPUParticles2D = $GrowingParticles
	@onready var growth_cycle_component: GrowthCycleComponent = $GrowthCycleComponent
	@onready var hurt_component: HurtComponent = $HurtComponent
	@onready var harvest_component: HarvestComponent = $HarvestComponent
	@onready var damage_component: DamageComponent = $DamageComponent
	@onready var digging_particles: GPUParticles2D = $DiggingParticles
	@export var cell: Vector2

	var growth_state: DataTypes.GrowthStates = DataTypes.GrowthStates.Seed

	var collectable_name: String = "cornseeds"

	func _ready() -> void:
		watering_particles.emitting = false
		growing_particles.emitting = false
		wet_particles.emitting = false
		
		hurt_component.hurt.connect(on_hurt)
		harvest_component.harvest.connect(on_harvest)
		growth_cycle_component.crop_maturity.connect(on_crop_maturity)
		growth_cycle_component.crop_harvesting.connect(on_crop_harvesting)
		DayAndNightCycleManager.time_tick_day.connect(on_time_tick_day)
		damage_component.max_damage_reached.connect(on_max_damage_reached)


	func _process(delta: float) -> void:
		growth_state = growth_cycle_component.get_current_growth_state()
		sprite_2d.frame = growth_state
		
		if growth_state == DataTypes.GrowthStates.Mature:
			growing_particles.emitting = true

	func on_harvest(hit_damage: int) -> void:
		if growth_state == DataTypes.GrowthStates.Mature:
			on_crop_harvesting()
		else:
			damage_component.apply_damage(hit_damage)
			await get_tree().create_timer(0.4).timeout
			digging_particles.emitting = true

	func on_max_damage_reached() -> void:
		await get_tree().create_timer(0.4).timeout
		call_deferred("add_seeds_scene")
		print("max damage reached")
		queue_free()

	func add_seeds_scene() -> void:
		var seeds_instance = seeds_scene.instantiate() as Node2D
		seeds_instance.global_position = global_position
		get_parent().add_child(seeds_instance)

	func add_harvest_scene() -> void:
		var harvest_instance = harvest_scene.instantiate() as Node2D
		harvest_instance.global_position = global_position
		get_parent().add_child(harvest_instance)

	func on_hurt(hit_damage: int) -> void:
		if !growth_cycle_component.is_watered and growth_state != DataTypes.GrowthStates.Mature:
			growth_cycle_component.is_watered = true
			watering_particles.emitting = true
			await get_tree().create_timer(5.0).timeout
			watering_particles.emitting = false
			wet_particles.emitting = true
			await get_tree().create_timer(20.0).timeout
			wet_particles.emitting = false

	func on_time_tick_day(day: int) -> void:
		wet_particles.emitting = false
		if growth_cycle_component.is_watered:
			growing_particles.emitting = true
			await get_tree().create_timer(1.5).timeout
			growing_particles.emitting = false
			growth_cycle_component.is_watered = false

	func on_crop_maturity() -> void:
		growing_particles.emitting = true


	func on_crop_harvesting() -> void:
		InventoryManager.add_collectable(collectable_name, 2)
		call_deferred("add_harvest_scene")
		queue_free()
		await get_tree().create_timer(1.5)
		growing_particles.emitting = false
	```








``` gdscript title="corn.gd"
func on_harvest(hit_damage: int) -> void:
	if growth_state == DataTypes.GrowthStates.Mature:
		on_crop_harvesting()
	else:
		damage_component.apply_damage(hit_damage)
		await get_tree().create_timer(0.4).timeout
		digging_particles.emitting = true
```

Adding a seeds counter to the toolbar, and making it so that when a crop is harvested the player gets some seeds back automatically, so you don't slowly run out of seeds. Also tweaking the inventory manager global and collectable component so that items can give multiple of themselves, meaning a mature crop now gives you back 2 seeds instead of 1.

### 08/01/25
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
