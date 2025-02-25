# 344 - Using a Game Engine
## Final Product
![type:video](https://drive.google.com/file/d/1CHvWJf9wxp5L5flG-RdlsD7bMdtc-yGf/preview)
## Software & assets used

| **Game engine:**        | Godot 4.3                                                                                             |
| ----------------------- | ----------------------------------------------------------------------------------------------------- |
| **Sprites & tilemaps:** | Sproutlands                                                                                           |
| **Fonts:**              | Sproutlands, ZX Palm                                                                                  |
| **Music:**              | later                                                                                                 |
| **Reference:**          | [Youtube](https://www.youtube.com/watch?v=it0lsREGdmc&lisYoutubet=PLWTXKdBN8RZe3ytf6qdR4g1JRy0j-93v9) |

## Self Evaluation
***What is the project you have been working on this term?***
> Creating a game mechanic for a farming game.

***Do you think you successfully did this?***
> I don't know my head hurts

***What parts of your project do you like?***
> Her beautiful smile

***What could you improve on, and what part would you do differently next time?***
> I would make the best game in the world ever evre erveer evrer ever ever ever EVER

***What part of the work did you find the most difficult and why?***
> I don't remember (truth)

## Devlog
### Test scenes & Tilemap
##### 18/11/24
Starting work on a farming game with a tutorial. There was a project before this but the tutorial was a full major version behind making development super slow (also I didn't like how the mechanics worked) so I did this one instead.

Created a game tilemap using the Sproutlands assets and made a test scene to... test it in, and started learning how to use terrains for things like paths and grass, so they can be dynamically added/"drawn" into the scene which will speed up and simplify level creation somewhat.

![](https://drive.google.com/thumbnail?id=1bVD0oc5EqaHO0BNeRb_R3EkCkbmY_7ZK&sz=s4000)
/// caption
Tilemap test scene
///

![](https://drive.google.com/thumbnail?id=1dAxDKQovlPeLOop76BL5DRZCoK0hwUzL&sz=s4000)
/// caption
Terrain testing for paths
///

### Player Animations & State Machine
##### 19/11/24
Created a player scene, and added animations for walking, idling, chopping, tilling and watering in 4 directions as well as some collision.

![](https://drive.google.com/thumbnail?id=1TwTseucj6G6mMkeNhEHJpOjOV_vzVIIG&sz=s4000)
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
Found the cause of the animation bug! The `idle_state` is supposed to get the player's direction from `walk_state` and act according to that, but I had a line of code that made it set the player's direction during their idle state to match the current movement input, which would obviously be none/null as no actions are being pressed in idle, so it always defaulted to the front-facing animation rather than following the player's actual last direction.
 
![](https://drive.google.com/thumbnail?id=1CrowXFbg4PYs_oMf6sLh8VK7n3GYW5Wa&sz=s4000)
/// caption
Staring really hard at `idle_state` for 2000000 hours to figure out what I messed up (ASMR) 
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

![](https://drive.google.com/thumbnail?id=1_O2XWQcHR3tk8JTuPZD9A3zU2NpEAZJz&sz=s4000)

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
![](https://drive.google.com/thumbnail?id=1a1CbH7GK-NY303pN0hntPtlluQ9Xgi6f&sz=s4000) 
![](https://drive.google.com/thumbnail?id=1nQr2RCerztyuMW__LrD0VR4OHaP2A6AO&sz=s4000)
/// caption
Adding the new house tileset, with layers for the floor, walls and furniture plus collision for objects and walls
///

![](https://drive.google.com/thumbnail?id=1Soja3q8RcjP63-q1Vjh1hCMXh2UQCiqf&sz=s4000)![](https://drive.google.com/thumbnail?id=1AXPkYU4TuQC8F4XTTFFpqld17ac2COhy&sz=s4000)
/// caption
HELL YEAAAAAAAAAAAA
///


Created doors using the same method of making a separate scene and then adding it to the tileset with scene collections, because they'll need a lot more functionality than a regular tile. They use an animated sprite, with only one animation, because for a smooth opening/closing transition even if the player quickly passes by before it fully opens, it's better to just reverse the animation rather than play a new one.

To make the doors actually functional, they use a component; essentially reusable scenes with scripts that contain a basic function (being collected, growing, taking damage etc) and can more or less just be tossed onto a scene to give it that functionality, then specialised and extended as needed. The component the doors use is an `interactable_component` that will allow the door to detect when it's being interacted with; in this case, when the player approaches.

![](https://drive.google.com/thumbnail?id=1nNgjARQw1wc6mQShbGRm6ibrsa7xuar8&sz=s4000)
/// caption
Door scene. Teal box is for detecting the player, pink is for wall collision, and the blue one (hidden under the teal one) is enabled and disabled to let the player in or out.
///

![](https://drive.google.com/thumbnail?id=1il99rECwUD4LCTK-IrKlMwqW3cmPHN8K&sz=s4000)

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


 
##### 02/12/24
Added some small trees. Since they need to be chopped down and drop items, they're being added to the tileset using scene collections as well. Created a few new components; `hit_component` and `hurt_component` for the player and environment/object hitboxes respectively, `damage_component` for taking/inflicting damage and `collectable_component` for collectable items. The trees each have their own HP, and the player's actions will now move a hitbox in front of them which can deal damage to objects whose type matches the player's current tool, disappearing at 0 and dropping an item that can be picked up. There is no inventory system yet though, so they just kinda disappear.

![](https://drive.google.com/thumbnail?id=1iHszOKQ-Id3-rpyENMZdtV2E3BgexZlq&sz=s4000)

LOGCHAMP!!!!!!!! ![](https://drive.google.com/thumbnail?id=1iHszOKQ-Id3-rpyENMZdtV2E3BgexZlq&sz=s40)![](https://drive.google.com/thumbnail?id=1iHszOKQ-Id3-rpyENMZdtV2E3BgexZlq&sz=s40)![](https://drive.google.com/thumbnail?id=1iHszOKQ-Id3-rpyENMZdtV2E3BgexZlq&sz=s40)![](https://drive.google.com/thumbnail?id=1iHszOKQ-Id3-rpyENMZdtV2E3BgexZlq&sz=s40)![](https://drive.google.com/thumbnail?id=1iHszOKQ-Id3-rpyENMZdtV2E3BgexZlq&sz=s40)![](https://drive.google.com/thumbnail?id=1iHszOKQ-Id3-rpyENMZdtV2E3BgexZlq&sz=s40)

The `hit_component` in the player scene is given its area by a small collision shape just for tools that is disabled by default, and each of the action states have been modified slightly to enable this hitbox and move it to the direction of the player. The component script gets the player's current tool, and how much damage they're supposed to deal, each as variables.

![](https://drive.google.com/thumbnail?id=1ltQWiIiBw7PSknLWCRIrCwcRykSnTzHa&sz=s4000)


The trees likewise have their `hurt_component` with its own collision shape and a specified tool to look out for; when the `hit_component` enters the `hurt_component` with the matching tool selected, it emits a signal containing how much damage the player deals. This signal gets picked up by the `damage_component` on the trees which handles their HP, and the component decreases their current HP accordingly. Once their HP hits 0, `damage_component` emits a different signal, and the `tree` script receives that signal and places a log scene before politely disappearing.

![](https://drive.google.com/thumbnail?id=1omakKfplls6v4KoK1Veyp1DslNhKyaRG&sz=s4000)
/// caption
Tree! (purple box is for the hurt components, blue is for basic  collision)
///

The log is simple, it has a `collectable_component` with a shape that looks out for the player, and once the player's main hitbox enters that shape, the component kicks the log out of the scene.


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

	func _on_body_entered(body: Node2D) -> void:
		if body is Player:
			print("Collected:", collectable_name)
			get_parent().queue_free()
	```

 
##### 03/12/24
Added a larger tree with some more HP, as well as a rock that uses all the same components but drop stones instead.
![](https://drive.google.com/thumbnail?id=18w4_IIhEizySahol2f_w1jjpARRxsMsn&sz=s200) ![](https://drive.google.com/thumbnail?id=1qud4O9Ml2FVN0mHC6ObeGmkfg-vsxmkQ&sz=s200)
/// caption
the destructible object brothers
///


Added an animation to all three destructible objects using shaders. Now when the player hits a tree or rock, they'll shake in response to it. There was a little issue where hitting one of an object would shake all of the others, but just ticking the "local to scene" box on the shaders solves that.

![](https://drive.google.com/thumbnail?id=1xa1bjXwRBLl7AU_Ey6qVV-66Qj2Xx-qp&sz=s4000)


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

### NPCs & Pathfinding

##### 04/12/24
Adding Y-sorting to the world and tilemaps. It's just a bunch of ticking Y sort boxes lmao. 

Starting work on chicken NPCs with their own animations and state machine similar to the player, with a NavigationAgent2D node that will be used for pathfinding. A NavigationRegion2D in the scene they're in, and on the same navigation layer as them, determines where the chickens are able to path to (defined by a NavigationPolygon), and their state machine should make them cycle between walking around their NavigationPolygon and idling.

Unfortunately, the chickens are currently a bit stupid. They're running into walls, endlessly walking (transition into idle state isn't working yet), bumping into each other, etc and all of them start walking at the same time on game start so they look goofy. 

![](https://drive.google.com/thumbnail?id=1i93vnLBxN65g04oQWFl4yD-cg38IUVMU&sz=s4000)
/// caption
chimken
///

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

![](https://drive.google.com/thumbnail?id=1MILDF-gyXvvLqTZi_sIRjZhFIU6Jhfji&sz=s4000)
/// caption
Chickens navigation, note how the shape automatically avoids fence tiles and the chicken paths filter through the only exit to get out, rather than just walking into the fences.
///

### UI for Game Mechanics

##### 10/12/24
Tweaked the cows a little to make them a little less active than chickens, now moving on to the game UI. Added a toolbar that lets the player equip the different tools, and an inventory bar that displays all the items and their count. The UI elements are styled by a game theme, with different custom "types" for the various types of elements, so all the tool buttons can be edited at once or have specific overrides.

Also added new collectibles like milk, eggs, wheat, tomatoes, and seeds using the same collectable component I made the rocks and logs from :)

![](https://drive.google.com/thumbnail?id=15XK9KSBh2L_a_eiiunr4ivHg5pClyyQq&sz=s4000)
/// caption
Themed inventory/tool bars
///

![](https://drive.google.com/thumbnail?id=1iKMa8AdiWGfcbtE8wRAOEQz6FBrlWR3e&sz=s4000)
/// caption
Collectable items currently in the game
///

#####  11/12/24
Finishing the inventory panel by adding custom fonts and item counters, then finally making collectables do something with the use of a global script that manages the inventory and modifying the collectable component so it adds items. I'll go over the scripts later, as they get a small rework pretty soon.

Adding a time mechanism along with new panels showing the current day and time, with some speed options so you can speed up the day. There was an issue where the time didn't update because I forgot to actually use the recalculate_time() function so it... never recalculated the time. Mindblowing.
![](https://drive.google.com/thumbnail?id=1ZxpYGwOGj-OcCIoefqAYxdQrVQn-Q0lX&sz=s4000)
/// caption
Day/time UI
///

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
![](https://drive.google.com/thumbnail?id=1A2m_e0o9qdQZwXZtt2kT1toNakuqsl0k&sz=s4000)

Completed the day-night cycle with a component to control the initial day and time that also adds a filter to the game screen with a custom gradient that changes over the course of a day, so as the day progresses the game appears to have a sunset, sunrise and night-time.

show time progressing video

Added some corn crops and gave them various particle effects for when they're actively being watered, wet, and mature. They use the same hurt component from the trees and rocks (listening for the watering can this time) to determine when they're being watered, and a new growth component controls their growth (WOAH NO WAY).

![](https://drive.google.com/thumbnail?id=1Un0pvOcuGWD4PZXDtHZEikoEFBH_WXW4&sz=s4000)![](https://drive.google.com/thumbnail?id=1ne40QqzswvnXyISCloVU6XQlLMKhPazn&sz=s4000)![](https://drive.google.com/thumbnail?id=1wFmgGH3zccRMm1V4nidLPW81Z19tvX0F&sz=s4000)
/// caption
Particle effects for freshly watered, watered and growing crops.
///



They're a bit buggy at present, and I don't particularly like how the tutorial handles crops. As it stands, when they're watered once they don't ever have to be watered again, but the wet particles disappear after a while so you can't tell which crops are watered. They will "harvest" themselves automatically after a day, which leaves the player with not much to actually do, and gives the impression the goods are just sitting on the ground being nibbled on by bugs all night. 

Additionally, the final harvested stage, despite looking like the item, actually isn't one and is uncollectable. They also look kinda boring when they just pop into the next growth stage, and the way the growth cycle is done means that even with a growth length of 5 days for the 5 visible stages, the sprites don't update predictably, so two crops that look the same may be harvested on different days. There's a lot to fix, but I'll just move on for now.

#####  18/12/24
![](https://drive.google.com/thumbnail?id=1G2XKibDUwmF2twZgEnfXn8tQaCTUDx3D&sz=s4000)

Finished the crops part of the tutorial, adding some tomatoes that work the same as the corn, then moving back to Y-sorting for a bit to add different origin points for the various tiles and objects like crops, so they sort from a point that looks believable for their sprite.

show y sorting

Added a new terrain for tilled dirt and started work on a script and component that will let the player add farmland by using the hoe on nearby grass tiles.

#####  06/01/25
Fixed a visual issue where tomatoes specifically would be Eternally Wet because it looked weird (even if I dislike not knowing which crops are watered...), turns out I forgot to connect a signal so they ignored the day ticking over. Also made it so that when a plant grows it emits the "maturity" particles for a short while so it's less boring. There's no internet right now, so I have to get silly with it. Tomorrow I will add subway surfers and family guy funny moments to the UI o7

##### 07/01/25
JUMPSCARE!!! It's a crops rework because I can't take it anymore. Crops now need to be watered every day, progressing one stage a day with each stage being directly correlated with a new sprite, and once they're mature they stop growing and wait to be harvested. On that note, I also added a new component specifically for harvesting crops (the hurt component is already being used for watering, and I'd rather not mess with the existing function too much and hurt my brain, so a new component it is), so now the player can use the hoe to harvest crops when they're mature. Added a damage component and a new harvesting function as well, so that prior to crop maturity, the player can dig up crops and have them drop a collectable that returns the seeds used, with some cute lil digging dirt particles to go with it.

![](https://drive.google.com/thumbnail?id=1EbfjDVebSS0IcgkWQYwz4FSRyJnpByN1&sz=s4000) ![](https://drive.google.com/thumbnail?id=1jk-XvYqRtJW2QxLql0eHDhYsroXihtAL&sz=s4000)

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
		InventoryManager.add_collectable(collectable_name)
		call_deferred("add_harvest_scene")
		queue_free()
		await get_tree().create_timer(1.5)
		growing_particles.emitting = false
	```

Added a seeds counter to the toolbar for each "seeds" tool, and making it so that when a crop is harvested the player gets some seeds back as well, so you don't slowly run out of seeds. Also tweaking the inventory manager global and collectable component so that items can give multiple of themselves, meaning a mature crop now gives you back 2 seeds instead of 1.

??? example "Changes to collectable items"

	The inventory manager global just looks for a second argument with the number of items being added or removed. Pretty shrimple

	``` gdscript title="inventory_manager.gd"
	func add_collectable(collectable_name: String, collectable_count: int) -> void:
		inventory.get_or_add(collectable_name)
		
		if inventory[collectable_name] == null:
			inventory[collectable_name] = collectable_count
		else:
			inventory[collectable_name] += collectable_count
		
		inventory_changed.emit()
	func remove_collectable(collectable_name: String, collectable_count: int) -> void:
		if inventory[collectable_name] == null:
			inventory[collectable_name] = 0
			return
		else:
			if inventory[collectable_name] > 0:
				inventory[collectable_name] -= 1
		
		inventory_changed.emit()
	```
	Wherever an item needs to be added or removed from the player inventory, now it's possible (and necessary) to specify the count. The collectable component has this as an export variable so it can be easily changed for each item, but the crops just give you 2 seeds back when harvested because that's all they need to do.

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

	``` gdscript title="corn.gd"
		func on_crop_harvesting() -> void:
			InventoryManager.add_collectable(collectable_name, 2)
	```

#### 08/01/25

Some more QoL stuff. Due to the way the inputs were handled in the tutorial, clicking on any UI element would trigger an action (chopping, tilling etc) before, and I don't like that. For instance, if you're holding the hoe in front of a crop and go to switch to the watering can, the player will damage the crop first, which is annoying. To fix this I moved the clicking actions into `_unhandled_input()`, giving them a lower priority than before. Now, because the UI handles the inputs first when you click on it, it doesn't count ad an unhandled input so the player won't start jopping instead of. 

There was a bit of a problem where the player would "double act", due to how I implemented the ability to transition into actions while walking (both scripts would detect the input as both scripts had the action transitions in them, and I've just moved them out of their state-specific functions so they're catching the input twice), and I used a quick fix of just adding a variable to each script that changed to true/false depending on whether their respective states are active, then tossing it onto the conditions for each action so that only one of the two main states can transition to an action at any time. That sounds convoluted but look it's literally just...

``` gdscript title="walk_state.gd"
var state: bool

func _unhandled_input(event: InputEvent) -> void:
	if  Input.is_action_just_pressed("hit") && player.current_tool == DataTypes.Tools.AxeWood && state == true:
		transition.emit("Jopping")
	elif Input.is_action_just_pressed("hit") && player.current_tool == DataTypes.Tools.TillGround && state == true:
		transition.emit("Tilling")
	elif Input.is_action_just_pressed("hit") && player.current_tool == DataTypes.Tools.WaterCrops && state == true:
		transition.emit("Watering")
```

####  13/01/25
Continued work on the tilling/placing crops mechanic, finishing the scripts for the field cursor component and crops cursor component; they are *meant* to place down tiles, but it's not actually placing them for some reason.

####  14/01/25
Found what was causing the issue, it was that the tilled soil layer didn't have a tileset assigned to it... Better that than a complicated bug I guess. Added some more tiles to the tilled soil terrain and fixed the bitmasks a bit, since it turns out the way they set it in the tutorial doesn't allow for certain shapes like a T shape... and I messed up the bitmask before. But now it's cool :) You can also un-till soil by pressing right click, and crops are only able to be planted on tilled ground so you gotta make little fields first to get planting.

I also made the seeds actually do something! So now you only plant crops if you have enough, and each time you plant a crop, the respective seeds go down by 1. That was actually surprisingly hard, there was a bug where if you started the game and tried to plant seeds without ever picking up or losing any it'd freak out because it was drawing from a null value. I fixed it by making the inventory manager "add" 0 to the items on start, so that if they're null they get assigned a value but if you already have some items it won't change that.

show planting/tilling in action xd


??? example "Planting/tilling mechanics"
	``` gdscript title="field_cursor_component.gd"
	func _unhandled_input(event: InputEvent) -> void:
		if event.is_action_pressed("hit"):
			if ToolManager.selected_tool == DataTypes.Tools.TillGround:
				get_cell_under_mouse()
				add_tilled_soil_cell()


		elif event.is_action_pressed("release_tool"):
			if ToolManager.selected_tool == DataTypes.Tools.TillGround:
				get_cell_under_mouse()
				remove_tilled_soil_cell()

	func get_cell_under_mouse() -> void:
		mouse_position = grass_tilemap_layer.get_local_mouse_position()
		cell_position = grass_tilemap_layer.local_to_map(mouse_position)
		cell_source_id = grass_tilemap_layer.get_cell_source_id(cell_position)
		local_cell_position = grass_tilemap_layer.map_to_local(cell_position)
		distance = player.global_position.distance_to(local_cell_position)
		
		print("Mouse position: ", mouse_position, " Cell position: ", cell_position, " Cell source id: ", cell_source_id)
		#print("Distance: ", distance)

	func add_tilled_soil_cell() -> void:
		if distance < 20.0 && cell_source_id != -1:
			tilled_soil_tilemap_layer.set_cells_terrain_connect([cell_position], terrain_set, terrain, true)
			#print("balls")

	func remove_tilled_soil_cell() -> void:
		if distance < 20.0 && cell_source_id != -1:
			tilled_soil_tilemap_layer.set_cells_terrain_connect([cell_position], 0, -1, true)
	```

	The crops script is much the same, with the main difference being the conditions for planting crops VS tilling land and the fact that it adds the crop scenes as instances rather than just adding tiles to the tilemap.

	``` gdscript title="crops_cursor_component.gd"
	func add_crop() -> void:
		var all_child_nodes = get_parent().find_child("CropFields").get_children()
		var is_cell_occupied: bool = false
		
		for node: Node2D in all_child_nodes:
			if node.cell == local_cell_position:
				is_cell_occupied = true
		
		if distance < 20.0 && is_cell_occupied != true && cell_source_id != -1:
			print("attempting to plant on: ", cell_source_id)
			if ToolManager.selected_tool == DataTypes.Tools.PlantCorn &&  InventoryManager.inventory.cornseeds > 0:
				var corn_instance = corn_plant_scene.instantiate() as Node2D
				corn_instance.global_position = local_cell_position
				get_parent().find_child("CropFields").add_child(corn_instance)
				InventoryManager.remove_collectable("cornseeds", 1)
				print("Planted corn")
			
			elif ToolManager.selected_tool == DataTypes.Tools.PlantTomato && InventoryManager.inventory.tomatoseeds > 0:
				var tomato_instance = tomato_plant_scene.instantiate() as Node2D
				tomato_instance.global_position = local_cell_position
				get_parent().find_child("CropFields").add_child(tomato_instance)
				InventoryManager.remove_collectable("tomatoseeds", 1)
				print("Planted tomatoes")
	```

	Oh and here's the bugfix, shrimple huh?
	
	``` gdscript title="inventory_manager.gd"
	func _ready() -> void:
			add_collectable("tomatoseeds", 0)
			add_collectable("cornseeds", 0)
			inventory_changed.emit()
	```


Went back to ignoring the tutorial for a bit to make tools de-selectable, I just don't like that once you select a tool you must bear that cross forever. And being able to not have tools in your hands means maybe... one day... I can make it so you get to pet the cows and chickens... surely.........

??? example "Dumb tool fix yaaaaaay"

	Each of the tool buttons now work as a toggle rather than an "on pressed" action. This bit is just for the axe, but it's the same for all of them.

	``` gdscript title="tools_panel.gd" hl_lines="5"
	func _on_tool_axe_toggled(toggled_on: bool) -> void:
		ToolManager.select_tool(DataTypes.Tools.AxeWood)
		if toggled_on == false:
			print("deselected axe")
			deselect_tool()
	```

	Toggling a tool button off will release every button from focus and set the current tool to none!

	``` gdscript title="tools_panel.gd"
	func deselect_tool() -> void:
	#func _unhandled_input(event: InputEvent) -> void:
		#if event.is_action_pressed("release_tool"):
		ToolManager.select_tool(DataTypes.Tools.None)
		tool_axe.release_focus()
		tool_hoe.release_focus()
		tool_water.release_focus()
		tool_corn.release_focus()
		tool_tomato.release_focus()
	```

 

### Saving & Loading

##### 15/01/25
Starting to add a save system, using P as a shortcut to save and adding various components that will allow the game save certain parts of the scene, such as the player-modified tiles and objects such as farmland and crops. It's a lot of scripting basically...................................................................................

There's also a bug where you can plant multiple crops on one tile, and I tried to fix it but it hasn't worked. THERE IS NO BUG. LOOK AWAY I STILL HAVETN FIXED I-

##### 20/01/25
Apparently I forgot to add collision keeping the player from walking off the island this whole time, so I did that lol. Started making the actual main level; it's implemented in a way that would allow for easy swapping of levels, though that functionality won't be used since the game only needs one environment.

The chickens are stupid again. I don't know why.
Y sorting doesn't work when using the main game screen, and I also can't figure out where the culprit is.

Started making a menu screen, except all I got to do was add 1 (one) unstyled button. We'll get there chat

##### 21/01/25
Finished the title/pause menu, which you can bring up using ++esc++ . It has options to Start, Save and Quit which all technically work, save for some bugs relating to saving and loading. The background of the menu is just another game scene, but frozen by setting the process mode to disabled so it's just a static background. It's not decorated yet, but that'll come later.

##### 22/01/25
It's later. I decorated the menu background so it looks cute now! Yay!
![](https://drive.google.com/thumbnail?id=1gUptbcAcMup_EhMJlq2hV8PkEGFBc4SI&sz=s4000)

##### 27/01/25
Added pausing functionality using get_tree().paused and setting the menu's process mode to always, so the entire game will freeze on pause except for the pause menu itself since the buttons still need to work, which is way easier than pausing things individually and potentially missing stuff. 

??? example "Pause menu functionality"

	``` gdscript title="game_manager.gd (global)" hl_lines="4 5 8 10"
	# Pulls up the menu when ++esc++ is pressed

	func _unhandled_input(event: InputEvent) -> void:
		if event.is_action_pressed("game_menu"):
			show_game_menu_screen()

	func show_game_menu_screen() -> void:
		var game_menu_screen_instance = game_menu_screen.instantiate()
		get_tree().root.add_child(game_menu_screen_instance)
		get_tree().paused = true
	```

	``` gdscript title="game_menu_screen.gd" hl_lines="3"
	# Sets the menu screen to always be processed, so it still functions while the game is paused
	func _ready():
		process_mode = Node.PROCESS_MODE_ALWAYS

	# These 3 functions all just connect their respective buttons to the scripts that handle their functions
	func _on_start_game_button_pressed() -> void:
		GameManager.start_game()
		queue_free()
		get_tree().paused = false

	func _on_save_game_button_pressed() -> void:
		SaveGameManager.save_game()

	func _on_quit_game_button_pressed() -> void:
		GameManager.exit_game()
	```

Saving and loading now properly works on main-game scenes, so I can go over the scripts now (later lmao)

??? example "Saving & loading functionality"
	``` gdscript title="game_manager.gd (global)"
	# Sets up the game and tells the other globals to load the base scene + any saved parts
	# Also sets a variable that allows the player to save now that there's a game to save
	func start_game() -> void:
		SceneManager.load_main_scene_container()
		SceneManager.load_level("Level1")
		SaveGameManager.load_game()
		SaveGameManager.allow_save_game = true

	# Quits the game (NO WAY!)
	func exit_game() -> void:
		get_tree().quit()
	```

	``` gdscript title="save_game_manager.gd (global)"
	extends Node

	var allow_save_game: bool

	# Lets the player save with just the key shortcut
	func _unhandled_input(event: InputEvent) -> void:
		if event.is_action_pressed("save_game"):
			save_game()

	# Tells the save level data component in the scene to... save the level data. Mindblowing
	func save_game() -> void:
		var save_level_data_component: SaveLevelDataComponent = get_tree().get_first_node_in_group("save_level_data_component")
		
		if save_level_data_component != null:
			save_level_data_component.save_game()

	# Tells the same component to load the data, only if there is any data to load
	func load_game() -> void:
		await get_tree().process_frame
		var save_level_data_component: SaveLevelDataComponent = get_tree().get_first_node_in_group("save_level_data_component")
		
		if save_level_data_component != null:
			save_level_data_component.load_game()
	```


??? example "Saving/loading components. (Mucho texto)"
	``` gdscript title="save_data_component.gd"
	class_name SaveDataComponent
	extends Node

	@onready var parent_node: Node2D = get_parent() as Node2D

	@export var save_data_resource: Resource

	func _ready() -> void:
		add_to_group("save_data_component")


	func _save_data() -> Resource:
		if parent_node == null:
			return null
		
		if save_data_resource == null:
			push_error("save_data_resource:", save_data_resource, parent_node.name)
			return null
		
		save_data_resource._save_data(parent_node)
		
		return save_data_resource
	```

	``` gdscript title="save_level_data_component.gd"
	class_name SaveLevelDataComponent
	extends Node

	var level_scene_name: String
	var save_game_data_path: String = "user://game_data/"
	var save_file_name: String = "save_%s_game_data.tres"
	var game_data_resource: SaveGameDataResource


	func _ready() -> void:
		add_to_group("save_level_data_component")
		level_scene_name = get_parent().name


	func save_node_data() -> void:
		var nodes = get_tree().get_nodes_in_group("save_data_component")
		
		game_data_resource = SaveGameDataResource.new()
		
		if nodes != null:
			for node: SaveDataComponent in nodes:
				if node is SaveDataComponent:
					var save_data_resource: NodeDataResource = node._save_data()
					var save_final_resource = save_data_resource.duplicate()
					game_data_resource.save_data_nodes.append(save_final_resource)


	func save_game() -> void:
		if !DirAccess.dir_exists_absolute(save_game_data_path):
			DirAccess.make_dir_absolute(save_game_data_path)
		
		var level_save_file_name: String = save_file_name % level_scene_name
		
		save_node_data()
		
		var result: int = ResourceSaver.save(game_data_resource, save_game_data_path + level_save_file_name)
		print("save result:", result)


	func load_game() -> void:
		var level_save_file_name: String = save_file_name % level_scene_name
		var save_game_path: String = save_game_data_path + level_save_file_name
		
		if !FileAccess.file_exists(save_game_path):
			return
		
		game_data_resource = ResourceLoader.load(save_game_path)
		
		if game_data_resource == null:
			return
		
		var root_node: Window = get_tree().root
		
		for resource in game_data_resource.save_data_nodes:
			if resource is Resource:
				if resource is NodeDataResource:
					resource._load_data(root_node)
	```

	
Also I secretly fixed logs accidentally being set to add 0 of themselves when collected (they were the first collectible so I glossed over them when adding collectable_count as a thing).

The game is basically done as far as the assignment is concerned, though I am putting off audio like my life depends on it because my headphones are too short :(

