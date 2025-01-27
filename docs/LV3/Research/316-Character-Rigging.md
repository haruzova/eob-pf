# 316 - Character Rigging
## Navigation
- [Bones & Joints](#bones-joints)
- [Weight Painting](#weight-painting)
- [IK & FK Chains](#ik-fk-chains)
- [Controllers & Constraints](#controllers-constraints)
----
## Bones & Joints
### <font color="#7359b3">(I) - What is the purpose of bones in a character rig?</font>
Bones, in the context of character rigging, are used to control the deformation of a character mesh. Similar to how bones work in our bodies, each bone is connected to certain parts of the mesh, and moving that bone will move the mesh accordingly. Bones are useful for having a consistent and simple way of animating or posing, so that rather than manually deforming the mesh for every movement, we can just move the appropriate bones instead.
## Weight Painting
### <font color="#7359b3">(I) - What is weight painting in a character rig?</font>
Weight painting is the process of creating a heatmap on a model, giving each vertex a "weight" that defines how much influence each bone of the model's skeleton will have on it. The end goal of weight painting is to achieve a result where all the movements made by the skeleton result in natural-looking deformation of the model that resembles how the object/body part would move in real life.
## IK & FK Chains
### <font color="#7359b3">(I) - What is the difference between IK and FK chains?</font>
A simple explanation of IK chains and FK chains would be that IK determines the position of all the bones in the chain based on a single target bone while FK involves moving each bone in the chain one by one. In practise, IK chains work somewhat like real chains, where the "weight" or IK bone at the end of the chain appears to drag the "links" or bones along, while FK chains work more like a robotic arm, with each bone rotating separately.

IK chains are useful for animating/setting poses where one bone needs to be kept in a particular place, such as a crouching animation for humanoids, where not only will you need to keep the feet planted to the ground while moving over half the model down, but correctly position the knees and legs, which is a lot more difficult to do with FK compared to using IK. On the other hand, creating good arcs and bold, sweeping movements with IK chains can be just as difficult if not sometimes impossible, since all IK does is "solve" the IK bone's position, but FK being a rotation-based system makes it the perfect fit for that use case.
## Controllers & Constraints
### <font color="#7359b3">(I) - What are controllers used for in a character rig?</font>
Controllers hold the original data of an object/part of a character model, such as the location, scale, and rotation. By changing the position of the controller, it's possible to move parts of the model without directly altering the mesh, making it a non-destructive change and meaning you can always return to the same original model. This is especially useful for animation, where a lot of movement and changes must be made without losing the original shape of the model.
### <font color="#7359b3">(II) - What are constraints used for in a character rig?</font>
Constraints in a character rig are applied to an object such as a bone in order to restrict their position, rotation, and/or scale based on other objects, or within a specified limit. The former is useful for animating objects that are supposed to appear attached to others, such as items being carried around, while the latter is especially useful for procedural or automated animation to achieve more accurate results without needing to impose those limits manually in post. It's even used in some 3D model tools for artist references such as in Clip Studio Paint, where users can set constraints on human models that mimic the real physical limitations we have, as to make creating a realistic pose easier. 