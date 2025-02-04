# Home
---
## **Level 2**
### **Units**
- **[202 - Digital Games Production](LV2-Units/202-Digital-Games-Production.md)**
- **[205 - Meeting a Digital Media Brief](LV2-Units/205-Meeting-a-Digital-Media-Brief.md)**
- **[207 - Making a Website](LV2-Units/207-Making-a-Website.md)**
- **[209 - Animation](LV2-Units/209-Animation.md)**
- **[210 - Creating Digital Art](LV2-Units/210-Creating-Digital-Art.md)**
- **[213 - Digital Audio Production](LV2-Units/213-Digital-Audio-Production.md)**
- **[217 - Marketing & Promoting Digital Media Projects](LV2-Units/217-Marketing-and-Promoting-Digital-Media-Products.md)**
- **[222 - Careers in Creative Digital Media](LV2-Units/222-Careers-in-Creative-Digital-Media.md)**
---

++ctrl+alt+del++

??? walk_state
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


> [!NOTE] Title
> Contents



## Level 3
### **Research**
- ~~Creating Video Games~~
- ~~Digital Concept art~~
- ~~3D Digital Art~~
- ~~3D Environments~~
- **[315 - Character Modelling](LV3/Research/315-Character-Modelling.md)**
- **[316 - Character Rigging](LV3/Research/316-Character-Rigging.md)**
- **[319 - Creating 3D Digital Animation](LV3/Research/319-Creating-3D-Digital-Animation.md)**
- **[323 - Creating Audio for Digital Media](LV3/Research/323-Creating-Audio-for-Digital-Media.md)**

### **Devlog/Projects**
- **[302 - Creating a Digital Game](Projects/302-Creating-a-Digital-Game.md)**
- **[316 - Character Rigging](Projects/316-Character-Rigging.md)**
- **[319 - Creating 3D Digital Animation](Projects/319-Creating-3D-Digital-Animation.md)**