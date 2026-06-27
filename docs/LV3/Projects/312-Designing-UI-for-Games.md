# 312 - Designing UI for Games
### Final Product
![type:video](https://drive.google.com/file/d/1A1CjTvqaTwOsCdgos2US9uKTlbCdeR1s/preview)

![BGMENU](https://drive.google.com/thumbnail?id=1PfZasbLxVJb3ETCZFXnrXB8jmztkmKAq&sz=s2000)
![BGSETTINGS](https://drive.google.com/thumbnail?id=10s3R-czVEG-Gky_Vc1B1WMfEsW7VyMBu&sz=s2000)

### Software and tools used

|                                   | -                                                                                     |
| --------------------------------- | ------------------------------------------------------------------------------------- |
| **Initial mockup, UI/BG assets:** | Clip Studio Paint                                                                     |
| **Game engine:**                  | Godot 4.x                                                                             |
| **Fonts used:**                   | [Little Guy by Maebunn](https://maebunn.itch.io/little-guy-font?download), [Kaph by GGBotNEt](https://ggbot.itch.io/kaph-font?download)|


### Self Evaluation
***What is the project you have been working on this term?***

> A mockup of a game's main/options menu.

***Do you think you successfully did this?***

> I think so...

***What parts of your project do you like?***

> I am quite happy with how the background turned out! Despite graphics being one of my weaker points, I was able to work around that by making use of vector layers and some simple texture brushes, and I could make the backgrounds and UI sprites in no time at all without it looking terrible! 

***What could you improve on, and what part would you do differently next time?***

> I'm not a big fan of how the spacing in the settings menu turned out. I'd have spent some more time getting things nice and balanced, and moved the title out of the way when the options are open. I would definitely theme the sliders to be chunkier and more readable, add styling to the popup menus, and design a menu opened via the start button for saving/loading or making a new world instead of just a start button, since I imagine this game would use a saves system.

***What part of the work did you find the most difficult and why?***

> I really struggled with node hierarchy and making everything space and fit together correctly. I tend to avoid UI/control nodes so making more than a few buttons and theming them all was a learning experience, and something I'm still not nearly fully versed in. I did want to make it in godot though, since there are some rules to structuring things that I'd like to follow, and I'm glad I did it that way!

---- 
### Thumbnail/initial mockup
##### 25/06/26
![mockup](https://drive.google.com/thumbnail?id=1SiVEWn0_0iJPlXxxfG_921OF6IEQxNZQ&sz=s2000)


![settings](https://drive.google.com/thumbnail?id=13I-IfrYDmbwtEWjZW25-ymyn7ieXetrE&sz=s2000)

I started making an initial mockup in CSP using just some basic blocking and brushes. The idea is that this is some kind of god-perspective game centered around a single planet at a time. Don't ask why there is a voice option. Maybe god has friends. You don't know that. You know what they say about assuming...

### Basic outline

##### 25/06/26
![ghost](https://drive.google.com/thumbnail?id=17nKuweRE9vw961UibXJJaNmHGUWdRPWd&sz=s2000)
![ghost fin](https://drive.google.com/thumbnail?id=1XFskOQJygftI9oHDxu20cqODCJUSz1jE&sz=s2000)


I lost the very very early screenshots, but this is all very basic menu stuff. I made a set of buttons, added some basic theming to my buttons, with a border and a hover/pressed version to warm up to Godot UI again, and then put a background onto it! I followed a tutorial to get navigation to and from the menus working. It's as simple as connecting on_pressed signals to a function that hides/shows the relevant containers.

??? example "Main Menu Script"
	``` gdscript title="main_menu.gd"
	extends Node2D
	
	
	func _on_play_pressed() -> void:
		$CenterContainer/MainButtons.visible = false
		$Back.visible = true
		$CenterContainer/SavesMenu.visible = true
	
	
	func _on_settings_pressed() -> void:
		$CenterContainer/MainButtons.visible = false
		$Back.visible = true
		$Settingscontainer.visible = true
		print("settings pressed")
		$AnimationPlayer.play("move")
	
	
	func _on_quit_pressed() -> void:
		get_tree().quit()
	
	
	func _on_back_pressed() -> void:
		$CenterContainer/MainButtons.visible = true
		$Settingscontainer.visible = false
		$Back.visible = false
		$CenterContainer/SavesMenu.visible = false
		$AnimationPlayer.play("moveback")
	``` 



```
```


### Making the mockup

##### 25/06/26

I drew some cloud sprites as a test, set them up to be the background for my buttons, and started to pick out fonts and colour for the text to get that handwritten feeling. 
![cake](https://drive.google.com/thumbnail?id=1oUv4XYIwf0Zi_TOSHEOH5rcmq3PaOSig&sz=s2000)

Using a solid rectangle as the underlying base in CSP and vector layers, I made some better clouds that have a clear space for text to go and look a little more clean. Using sub regions, I set each button to use a different cloud sprite, and found the fonts I want to use for the final thing! It's here where I decide my intial mockup's colours weren't to my taste, and I turned them blue to match the cloud shading.

![wip sprite](https://drive.google.com/thumbnail?id=1dJ_rXNYluXcepyDdxaZ9sWh9ug6D4xtD&sz=s500)

![wip sprite](https://drive.google.com/thumbnail?id=1w3uPlKKtmNssW-FJJG5HgmVnvuyKf_jI&sz=s2000)

On hover, the buttons will darken and essentially swap colour schemes, for contrast! I considered doing grey for a stormcloud effect, but I didn't like the look in the end.

![cloudhover](https://drive.google.com/thumbnail?id=1NiRHTydQcPMKNvmGLSnIzzYXQ_8Q08EK&sz=s200) AAAA

##### Settings

I started work on the settings, but not before tweaking the title a little to make it look like it's backlit in the same way as the planets! I thought it would add some nice depth.

There's not a lot to say here... The settings menu is a PanelContainer with a custom cloud sprite made in the same way as the buttons, and the back button is just a button. I'm sorting things with BoxContainers to organise them horizontally and vertically, and applying a little bit of styling to the elements to get the right visual idea going.

![settings](https://drive.google.com/thumbnail?id=18mKCVZn298MFOOCARKHmDkMwGF562nCC&sz=s2000)


##### Finishing up

I clean up the background sprites a little with some texture, cleaner shapes, and adding depth via shading, so they look nicer without being perfect rendered graphics. I put them on separate layers as well, so it'll be possible to move them separately.

![bgnew](https://drive.google.com/thumbnail?id=1ZS83iQvb9pyWNlK0bv2a85Jr4To1pPfe&sz=s2000)

??? example "Sprites"
	![sky](https://drive.google.com/thumbnail?id=1M3FDKRomdhBnc60Rim1ZKjwEoatRb75o&sz=s1000)
	![planets](https://drive.google.com/thumbnail?id=1OVCo7FD9aJgbpO-dYHdXW53xmiG8fGMP&sz=s1000)
	![world](https://drive.google.com/thumbnail?id=1ywjTY7qIVQ-ki1g-ptV5A1LSIFTNyVg3&sz=s1000)


You might've spotted it in the script earlier, but I did add some very very simple animations. They're nothing crazy and not polished for real use, but the point is to show how the background will zoom in while looking at the settings (and ideally the start/saves menu).

![BGMENU](https://drive.google.com/thumbnail?id=1Im8qSyUYIv58OsNZnYI13O1CojlLyBii&sz=s2000)
![BGSETTINGS](https://drive.google.com/thumbnail?id=1-kJlhKwVGkf9GeXsHqbAIa_pIagSrMJ3&sz=s2000)

Here's how everything turned out cropped!

![BGMENU](https://drive.google.com/thumbnail?id=1PfZasbLxVJb3ETCZFXnrXB8jmztkmKAq&sz=s2000)
![BGSETTINGS](https://drive.google.com/thumbnail?id=10s3R-czVEG-Gky_Vc1B1WMfEsW7VyMBu&sz=s2000)
