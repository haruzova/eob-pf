# 323 - Creating Audio for Digital Media
- [Audio Engineers](#audio-engineers)
- [Composers & Foley Artists](#composers-foley-artists)
- [Microphones & Audio Interfaces](#microphones-audio-interfaces)
- [DAWs (Digital Audio Workstation)](#daws-digital-audio-workstation)
- [Recording & Editing](#recording-editing)
- [Mixing & Post-Production](#mixing-post-production)
- [Sound Effects, Music & Voice](#sound-effects-music-voice)
- [Game Engine Implementation](#game-engine-implementation)
----
## Audio Engineers
### <font color="#7359b3">(I) - Explain the role of an audio engineer.</font>
Audio engineers (also known as sound engineers or technicians) are tasked with a number of highly technical and creative jobs in audio production; they are considered responsible for jobs such as recording, editing, mixing, refining and mastering audio, along with managing, maintaining and proficiently operating the audio equipment required for those jobs. 
### <font color="#7359b3">(II) - List 3 pieces of hardware an audio engineer might use and describe their purpose.</font>
#### <u>Microphones</u>
Microphones, or mics, are obviously necessary equipment for recording any audio to begin with, but in the field of audio engineering, the recording process is especially important, so multiple types of mics are used to fit with various contexts, recording locations, or to achieve different effects. For example, if the sound being recorded is from a loud and hectic live performance, the versatility of a dynamic mic would be preferable to the more delicate ribbon mic - but the reverse becomes true when trying to achieve the most natural-sounding recording in a studio. Having a suitable mic setup for the situation and desired final product will result in a much higher quality recording, which both saves time and leaves 'room' for additional changes in the editing/mixing stages.
#### <u>Mixing Console</u>
A mixing console, mixer, or soundboard can be understood as being a kind of main hub for audio; their basic purpose is to accept a number of audio signals, then process, balance, and most importantly combine them, before sending the result as output to where the user needs them. Many mixers are able to produce multiple outputs in this way, and most will come with a large number of sliders and inputs, providing a variety of functions useful for mixing such as volume control, panning, filtering, equalisation, and dynamic range compression. Some other useful functions of mixers would include providing phantom power for equipment that needs it, which all mixers can do, and acting as an amplifier for use cases such as live performance, which is only found in mixers designed to do so.
#### <u>Direct Input Boxes</u>
DI boxes, or direct input/injection boxes, are used by audio engineers when recording live audio. They convert the high impedance and often unbalanced signals outputted by most instruments to mic level (meaning the same signal strength/voltage as a microphone would give), making them compatible with mixing consoles which are best suited for handling mic level inputs. At the same time, DI boxes protect the signal from external noise as it travels, allowing audio engineers to run much longer cables from instruments without unwanted noise and loss of detail, thus eliminating a lot of the hassle in the live recording process.
## Composers & Foley Artists
### <font color="#7359b3">(I) - Explain what challenges a composer might face when making music for a game, as opposed to a movie or TV show.</font>
Creating music for video games introduces a number of challenges unique to the medium that video game composers have to work around or solve. For example, when creating a background track for a scene in a movie, a composer can simply create a track that suits the length and pacing of the given scene and be done; however, in video games, there is potentially no time limit to how long a player can spend in a particular place, so a composer has to ensure that those tracks loop smoothly and don't become repetitive too quickly.

Other challenges could include having to produce alternate versions of a track to fit the situation if the game has an adaptive soundtrack, and ensuring each track/track variant can piece together without sounding jarring - and composers wil need to keep all of these in mind to achieve a cohesive and effective soundtrack.
### <font color="#7359b3">(II) - Foley artists have to record sounds for different situations. Think of or find 3 scenarios where sound would need to be recorded, and how a foley artist might go about recording it.</font>
The sound of a creaky floor could be created by setting up a contraption coloquially known as a creaker, consisting of wood planks in a frame which can be bent in various ways to produce different sounds. Striking or waving sheet metal is a popular technique for replicating the ambient sounds of thunder, with thicker sheets and bigger impacts being ideal for closer-sounding lightning strikes. And for the visceral or gory impact sounds found in many video games, foley artists can destroy fruits such as melons with various tools for a more exaggerated and ethically sourced sound than real blood and guts.
## Microphones & Audio Interfaces
### <font color="#7359b3">(I) - Explain how a microphone is different from a speaker.</font>
The primary difference between microphones and speakers is that a microphone will accept soundwaves as input and convert it into electrical signals, while a speaker will take electrical signals representing audio and convert them to actual soundwaves.
### <font color="#7359b3">(II) - Do some research on 3 types of 'polar patterns' and explain in what scenarios they would best be used for audio recording.</font>
#### <u>Bidirectional</u>
Bidirectional patterns are useful for scenarios where recording two particular audio sources and little to nothing else is the goal, especially when those sources are on opposite sides of the mic or in too close proximity for two cardioid mics to be as effective. This could be for recording a conversation where all parties are either in front or behind the mic, such as for a two-person podcast or an interview. Another use case would be for miking a singer and their instrument at the same time, either using a bidirectional mic on its own or using it alongside a cardioid mic, each method making use of the fact that bidirectional mics have the highest sound rejection from the sides compared to other polar patterns to deliver the desired sound.
#### <u>Cardioid</u>
Cardioid patterns are popular for use in scenarios where isolating one particular sound source from background noise is desired, such as when recording in a noisy room or with multiple audio sources that need to be recorded separately. This can also be useful for avoiding feedback in a live performance scenario where the recorded sound is also being played out loud, as the low-sensitivity area behind cardioid mics can be safely used to place monitors without having them pick up that audio.
#### <u>Omnidirectional</u>
Omnidirectional patterns are ideal for scenarios where one wants to capture the whole sound of the area being recorded without favouring one particular spot, as well as scenarios where multiple audio sources are present and recording each separately is not the goal. Some examples of this would be for miking large sound sources such as from a grand piano or choir, recordings that need to include the ambient soundscape such as those of symphonic performances, as well as for recording meetings where multiple people may be speaking from lots of different directions or while moving around the room.
### <font color="#7359b3">(III) - Describe the function of an audio interface.</font>
An audio interface goes between an audio input and your computer, and is the place where all your audio inputs and outputs apass through and are processed. They perform both DAC and ADC (Digital-Analog Conversion and Analog-Digital Conversion), at a much better level than the soundcards built into most PCs, along with containing a preamplifier to help signals be picked up in a DAW. Those built-in soundcards are primarily made for playback, while a dedicated audio interface is built to handle professional use, allowing for more inputs and outputs, better audio quality, lower latency, and more specialised audio inputs and outputs.
## DAWs (Digital Audio Workstation)
### <font color="#7359b3">(I) - Briefly describe what a DAW (Digital Audio Interface) is. List 3 features typically found within a DAW (Digital Audio interface) and explain their function.</font>
DAW stands for Digital Audio Workstation, and is a type of software used in all types of audio-related fields as a kind of virtual studio to record, create and edit audio in. Most DAWs will have features such as:
#### <u>Multi-track Editing</u>
Multi-track editing is simply the ability to edit and play back multiple audio tracks, which is a core feature of many digital audio workstations. Each track in a DAW essentially serves as a "layer" of audio, which can be cut, moved around, and mixed as needed, even simultaneously. There is usually no limit to the amount of tracks in a project other than the processing power and space on the user's device, so multi-track editing is especially useful for particularly detailed and elaborate projects.
#### <u>Audio FX</u>
Audio Effects or FX consist of a number of processing tools that alter the way a piece of audio sounds. This includes compression, distortion, reverb, filtering, and EQ, which are the common types that come built-in to most DAWs, but a wide variety of different plugins also exist which can be installed and used to create all kinds of unique sounds and enhance the final product. Some effects can also help cover up issues with recorded audio, such as noise reduction effects and those that remove crackles, buzzes, pops or clipped off sections in audio.
#### <u>MIDI</u>
MIDI is short for "Musical Instrument Digital Interface" and is a standard for communicating and storing musical information that allows a user to use one single controller to use a wide range of virtual instruments and synths, as opposed to needing separate controllers for each one. MIDI functionality in a DAW will always include some way to sequence notes and play them back, either through a physical MIDI controller or with the software-included tools, and might also include tools for more intricate or optimised composition.
### <font color="#7359b3">(II) - Explain the difference between ‘Mixing’ & ‘Mastering’ as thoroughly as possible.</font>
The exact difference between mixing and mastering is somewhat hard to describe, since the purpose and process of both has changed a lot over time. In a broad sense, mixing is about taking the already determined structure of a song and manipulating it to achieve the desired sound, while mastering serves as a "final pass" where the last adjustments and tweaks are made to ensure the song is well-balanced and suitable for distribution. While the difference used to be more pronounced back when mastering was necessary in making a playable final product, the main and most salient distinction today is that of how close to completion the project is. Mixing is the first step after recording/composing, and involves making much more dramatic changes. It's here where the song gains 99% of its signature sound, and some preparations are typically made here to reduce the amount of time spent on mastering, such as testing the mix on different headphones while working. Mastering on the other hand is the final step, always done at the tail end of production and involving less drastic alterations. Here, the priority shifts from creating the overall sound to adding consistency, polish, and overall enhancing the final sound.
## Recording & Editing
### <font color="#7359b3">(I) - Briefly explain what the practice of ‘audio editing’ is and what methods/techniques are used when editing audio.</font>
Audio editing refers to any processes which involve altering a piece of audio to produce a desired sound. This might include techniques such as cutting and rearranging parts of an audio track, altering the length of a clip, enhancing the quality of a recording via something like EQ, or changing the sound itself with effects such as pitch shifting.
### <font color="#7359b3">(II) - What is the difference between ‘destructive’ and ‘non-destructive’ editing?</font>
Destructive editing is any editing that directly changes part of the file. Once a destructive edit is saved and no longer in the software's undo history, that change is permanent and the only way to revert it is through keeping and loading a backup of the original. Non-destructive editing, on the other hand, doesn't overwrite the original file, and can be reversed at any time. A lot of software makes all changes non-destructive unless the user chooses otherwise, making for larger file sizes but lowering the risk involved in audio editing.
## Mixing & Post-Production
### <font color="#7359b3">(I) - List and describe a minimum of 3 steps in audio post-production.</font>  
#### <u>SFX</u>
SFX, short for sound effects/"FX", is exactly what it sounds like - the part of audio post-production where sound effects are introduced and edited for cohesion. The effects might be made specifically for the production through techniques such as foley, or they might be taken from dedicated SFX packs.
#### <u>ADR</u>
ADR (short for either automated "dialogue replacement" or "additional dialogue recording" depending on the context) is a technique primarily found in film, where voice lines/dialogue are re-recorded in a more controlled setting, and adding it into a scene in post, either to replace low quality or missing dialogue, change the length/content of the dialogue, or for dubbing into another language.
#### <u>Dialogue Editing</u>
Dialogue editing is a step in audio post-production where all the recorded dialogue is cut, arranged, synchronised, corrected and cleaned up for the final print. This involves a lot of different processes, from cutting out filler words and removing mic pops and clicks, to enhancing the clarity of the recorded voice with de-essing and EQ.
#### <u>Music Arrangement</u>
Music Arrangement is part of music creation that happens after composition, where the concept or disparate elements of a song are brought together and arranged to create the first complete version that is ready for mixing and mastering.
### <font color="#7359b3">(II) - List at least 2 applications used for audio post production and at least 2 pros & cons for each. </font>
#### <u>Audacity</u>
**Pros:**

1. Free and open source
2. Relatively gentle learning curve
3. Straightforward and approachable to use

**Cons:**

1. Primarily a destructive editor
2. Somewhat unintuitive UI
3. Becomes clunkier to use the more advanced a project is
> NOTE:
>
> Depending on the user's preference, destructive editing may be ideal, so this is subjective as a con.

#### <u>AVID Pro Tools</u>

**Pros:**
1. Advanced features and automation
2. Excellent workflow
3. The industry standard for commercial productions working with live/recorded audio, being built from the ground up for such use cases

**Cons:**

1. Expensive
2. High barrier to entry
3. Comparably lacking in support for projects that use more samples or loops than recordings

----
## Sound Effects, Music & Voice
### <font color="#7359b3">(I) - Find 5 examples of good sound effects used in video games on youtube, add the link, say where it is from and explain why you feel this is a good sound effect.</font>
#### <u>Pikmin - Captured/Dying SFX</u>
The saddest example I have of good game audio implementation is in Pikmin. Whenever a Pikmin is snatched up or knocked down by an enemy, they'll immediately cry out; this is because they are aware of their mortality and fear it, but more importantly, it's a high pitched, sharp and urgent sound that signals to the player that it's their last chance to save that Pikmin, without any need for a tutorial. It's both instinctually attention-grabbing and adorable enough to make you feel obliged to help them, even if it's less strategic at times, a theme which itself becomes a part of the story, and certainly helps the player sympathise with Olimar. Doubly so if you fail to save some and have to hear their sad little ghostly sighs as they float away...
(https://youtu.be/Avjo5OGyS2Q?t=1h44m7s) 

#### <u>Minecraft - Ambient Music</u>
Minecraft's background music is a good example of a way to implement music into a slower-paced game (or a game that enables long play sessions) without making the soundtracks feel repetitive. The ambient music approach, where instead of constantly playing, the soundtrack occasionally shows up and hangs around for a while, not only limits repetition but helps the game's world feel more organic. A system like this can create great little moments where the music pops in just as you've achieved something, or stumbled across some awesome world generation. It helps that the OST itself is incredible, but the implementation doesn't let it down.
(https://youtu.be/yVe88ZBjYjA?t=40m24s)

#### <u>TLoU- Clickers</u>
The Last of Us' third stage of infected, Clickers, are some of the most dangerous enemies in the game. Their strength in combat and sensitive hearing make it so that as soon as a player knows they're facing clickers, they need to get low and stay quiet, and the way their audio is designed plays into that while maintaining a good horror atmosphere (partially by virtue of how creepy the sounds themselves are).

The signature "clicking" they make on patrol is both unique and easily distinguishable from all the other Infected, as well as varying in intensity and volume based on how alert the enemy is. For some specific examples, a sleeping Clicker is just quiet enough to catch a careless player off guard, making up for their reduced senses with ambush potential, while a patrolling Clicker who's just picked up a noise will grow louder and let out telegraphed "barks" as it starts using echolocation more proactively, making up for the larger immediate threat with an easy way to know where that Clicker is at all times. A good way to think about it is that the better you can hear a Clicker, the higher the odds are that it hears you too.
(https://youtu.be/aLg98S4sxsE?t=1h25m11s)
#### <u>FNaF - Freddy's Laughter</u>
Another example of horror games that use the SFX of an enemy in a way that's both creepy and serves a gameplay purpose would be Five Nights At Freddy's. There's a lot going on in that game that falls under this category (I particularly like the pathetic little clicks made by the door buttons when they're disabled, replacing the comfortingly solid shutting sound they otherwise make), I'd like to focus on none other than Frederick Fitzgerald Fazbear himself.

During gameplay, the player will occasionally hear an echoey, slowed down laughing sound play. Initially it seems like another "fan noise", something that's only there to unsettle you and keep you on edge, and it does that very well, but over the course of the game, more observant players will realise that Freddy's the one responsible for the noise. If you're watching him on the cameras you might notice that every time there's laughter, he'll move elsewhere. It's a small comfort and strategic advantage to be able to know when one of the animatronics is moving, while not cheapening the scare factor. Note that something similar is also done with Chica, where you'll always know when she's in the kitchen by the strange sounds coming from the kitchen camera.
(https://youtu.be/QXKLr6S7BOs?t=34m33s)

#### <u>Limbus Company - Kromer Battle Music</u>
At the climax and narrative finale of Limbus Company's third chapter, The Unconfronting (or just Canto III), players must face a boss battle against Kromer, who I shall merely explain as "Very plot significant to the chapter", lest I accidentally write the transcript of the entire thing, complete with annotations and analysis. The story until this point has mostly been told in between combat rather than during it, but Kromer's boss fight is a key sequence where they both must play out at once, and the implementation of the music in this encounter does a lot to help meet that end.

The music for this fight drops the usual soundtrack completely and instead features a lyrical song; that being [Between Two Worlds by Mili](https://youtu.be/lu8i-OyjLYk). This is the first time it happens in the game, and the sudden shift from the typical battle music into the unusually serene first half of Between Two Worlds is surprising and gives a sense of importance - that this is what the chapter's been building up to. The song itself is split into two sections to match the two phases of the fight, each section working to set the tone for their respective phase while the lyrics elaborate on the events and themes of the Canto, keeping the player conscious of all that's happened prior even during this final stretch. As the fight progresses past the first phase, and Kromer undergoes a horrific transformation, the music follows suit, gaining an instrumental backing that, while being an integral part of the sound in this latter half that contrasts it from the acapella of the first half, also sneakily allows the OST to transition into a non-lyrical version during dice-roll events and when the visual-novel storytelling has its cue to return. Overall, the marriage of gameplay and narrative in Canto III's finale is made more impactful because of how the OST is both written and implemented, and later Cantos still use a similar principle with their final boss encounter OSTs, though each one elaborates on it in a different way.
(https://youtu.be/UGkCLwz3YCk?t=20s)

(I tried very, very hard not to go off on a tangent about the story because I wasn't sure how much lyrical analysis you can fit in something before it stops being about the audio. There are like 10 discarded paragraphs about Sinclair in my notes here. 'Tis a horror I have been most merciful as to bear myself, in turn sparing thee.)


## Game Engine Implementation
### <font color="#7359b3">(I) - How could audio be used well in games to indicate a key event, such as low health or an enemy approaching which then makes a player respond to the event in the game? Give an example of this.</font>
Audio cues can be an ideal way to communicate key events in a game in times when there is too much going on on-screen for those events to be immediately obvious without it.

As an example, [Limbus Company](assets/misc/jiggyclam.gif), a turn-based strategy game, has an important mechanic known as a stagger, where most units will have one or multiple thresholds in their health bar that once reached will "stagger" the unit for a turn, leaving them both unable to act and vulnerable to damage. It can be useful (and satisfying) to the player to know precisely when and why an enemy/ally is staggered, but the complex nature of [Limbus Company](assets/misc/ishyprofen.png)'s combat means that relying on visual cues alone isn't enough to make this clear. (Some encounters can involve enemies with huge health pools, fast and chaotic combat phases, attacks that hit multiple units at once, scripted or conditional staggers that don't affect a unit's "real" stagger thresholds, etc...)

To emphasise staggers whenever they occur, a unique "breaking" sound effect is played the moment they happen, and allied units will play their "staggered" voice lines, so the player always knows when and why an ally/enemy is staggered the next turn, even in situations where the visual cues aren't clear enough. For a complicated and busy combat system where simply showing something on screen isn't enough to make it stand out, audio can be the perfect solution.
