# 303 - Creating and Publishing Digital Content

## Navigation
- [Design](#design)
- [Publishing](#publishing)
- [Content](#content)
----

## Design

### (I) - What is a games design document?
??? info "3D Character Artists - *(From **[315 - Character Modelling](LV3/Research/315-Character-Modelling.md)**)*"
    Game design documents are a kind of design document for game design and development that exist to define and illustrate the core concepts and ideas behind a game project. They often cover as many aspects of a game as possible, including things such as the core gameplay and mechanics, visual style, storylines, what kinds of /assets will be needed, and the development schedule/plans for release. It's important to create a game design document before seriously beginning development on a project, in particular for games made by multiple people, each who may have their own, conflicting creative vision.
### (II) - What is a design brief and what might be included in one?
??? info "Design Brief - *(From **[205 - Meeting a Digital Media Brief](/LV2-Units/205-Meeting-a-Digital-Media-Brief.md)**)*"
    A brief for game development is a document shared with developers that outlines the client's requirements and the core concepts and details of a game project.

More generally, design briefs serve as a very early preview of what the a game's design intention is. It might include ideas for the genre, story themes, aesthetics, core gameplay loop, and what kind of experience it is (think live service/multiplayer/single player, PC/console/mobile/VR, hardcore/casual/adjustable difficulty, or things such as game length.). There may be concept art or images included, and comparisons may be drawn to other games with relevant elements or design, all to better give the impression of the game's core ideas.

### (III) - What is the difference between a GDD and a design brief?
A good way to think about this is to look at what each document is used for. A design brief is used to show what the game *could* be, typically to non-developers, before it really exists as anything bigger than an idea, while a GDD is used throughout development and amongst the team working on it. Design briefs generally explain how the game is going to feel and look, without needing to cover details like damage formulas or niche mechanics. GDDs on the other hand are a lot more detailed, being intended for a team of developers to be able to work separately while being able to consult the document for some things without needing to check in with each other every time. As one example, a design brief may show a mood board to describe a level, while a GDD will preferably explain what level of progression the level should be built around, any key items that need to go there, and/or a sketch of the level itself and more specific details on how it should look (i.e.: add a bunch of big spooky tree models and ghost orbs!! everywhere!!!!!!) to make the design direction clear for everyone.

## Publishing

### (I) - What are some storefronts or platforms people can use to buy their games?
Some examples for places to share, download, and/or buy games would include: Steam or GOG for PC, PlayStation Store, Nintendo eShop for consoles, Google Play Store or F-Droid for Android, Apple App Store for iOS/iPadOS/MacOS, and itch.io for a variety of platforms including PC and Android.

### (II) - What are the pros and cons of some of these options?

<u>Note:</u> As should be fairly obvious, comparing storefronts/online platforms directly with each other like this is not going to be accurate for every project. Developers don't generally pick where to publish after making a completely universal game that could theoretically work with every OS without extra work, and compatibility is usually a primary factor that narrows down the options here. For this reason, I'm only comparing those that share a common supported platform (Platform here referring to the OS/device, not the online kind).

#### <u>Steam</u>
Steam is one of the most well known platforms for PC, being simple to use for both devs and consumers. However, it does have an upfront fee of 100 that isn't returned until the game makes 1000; something that may not happen for niche, free or cheap games. There's also a 30% minimum platform fee, cutting a bit more into dev income overall in exchange for access to a more mainstream audience.
#### <u>itch.io</u>
itch.io is smaller than the likes of Steam and as a result tends to get fewer eyes on your game depending on the genre and appeal of it, but is free to publish games to and has a 0% minimum platform fee, defaulting to 10%. There's also the option to donate when purchasing, meaning players can easily support a project/developer, even if it's free to download or under-priced. While it is more niche of a platform, there is a place for particular genres and developing a more particular audience. For horror games, RPGmaker games, visual novels, html games and other niches that have a large following on itch.io, publishing there helps the game's visibility much more than other platforms since that's where a lot of the fans of those genres will be looking for new games.

### (III) - Where would you post your game as an indie dev?
I would use itch.io, and/or shared as a file via my own site, torrent, or file hosting, as my priority is making my work accessible rather than visibility or features. If it is on PC and an ambitious enough project, I may consider Steam as well.
## Advertising

### (I) - What is an ad campaign?
An ad campaign is an organised way of putting a game out there and promoting it to audiences. This may involve making video advertisements, social media posts, posters, or events all centered around getting more eyes on the game. The main objective of using an ad campaign is trying to make sure as many people who might want to play your game know it exists and what it's about, ideally increasing popularity and sales or pre-registrations.

### (II) - If you were making an indie game how would you advertise your game & would you use an ad campaign?
For my own project, I would share the game's site/page on forums relating to indie games or the theme/genre/subject matter, as well as other online spaces I'm active in and amongst friends who may be interested. An ad campaign is overkill for the kind of projects I intend to do. Virtually everything creative I do is done purely as an artistic endeavour, as opposed to commissioned or hired work that's commercial in nature, and I'm not interested in leading a commercial game project whatsoever, so I would only be concerned with advertising in its most basic form, as a kind of show-and-tell. Promotional material would likely be limited to the game's page on storefronts/platforms, a trailer, and whatever supplementary art I make that references the game.

## Content

### (I) - What are content creators? What do content creators do?
Content creators are people (whether individual or as a group) who produce educational or entertaining material and share it with others, typically over the internet. The "content" can be videos, images, text, assets, or some believe software can be considered content in this manner. The role can go beyond just the process of creating the content in question, often also including tasks such as  managing accounts on social media, scheduling uploads, keeping up with a community, etc. on a case by case basis. 


# Practical (kind of)
### Software and tools used

|                            | -                                                                                     |
| -------------------------- | ------------------------------------------------------------------------------------- |
| **Static Site Generator:** | [MKDocs](https://www.mkdocs.org/)                                                     |
| **Hosting:**               | Github Pages                                                                          |
| **Writing/Designing:**     | [Obsidian](https://obsidian.md/), VSCode                                              |
| **Other:**                 | [Termux](https://termux.dev/), [PRoot-Distro](https://github.com/termux/proot-distro) |

### Self Evaluation
After suffering far too long at the hands of W\*x, and realising I was simply unable to edit any of my work at home on Android without losing years off of my lifespan, I remade my website from unit [207](LV2-Units/207-Making-a-Website.md) entirely. Copying and pasting everything out of Wix and into offline documents, I decided to focus on making the processes of creating the site, adding content, and reading that content as simple as possible. For that reason I chose to use MKDocs, which allows me to write my work down in any program, add basic Markdown formatting, and when finished, quickly make that .md file into a page of my site without any hassle. I tried Vercel at first, but hated it so much I no longer remember what it was like... I wound up using Github Pages as hosting instead, both because it's free to use for my use case (a single/primary site), and since I was working on the site both at home via Termux Linux and at school via Windows, I could just use github to move my work and keep it updated everywhere.

I struggled at first with getting Github Pages to function. `mkdocs serve` would work, but signing in to github to get `mkdocs gh-deploy` to work was a huge hassle at some point, and I blame VSCode for that because it also kept hanging when trying to commit, losing me the ability to move my work over without brute-forcing it via Syncthing. The first issue I fixed, though I forget how, but VSCode still won't work to commit my changes, so I switched to using github desktop for git related stuff, only using VScode for final page edits and locally serving/deploying the site. There have been no shortage of MKDocs quirks relating to links, as well. I had to begrudgingly switch to linking images and videos via google drive, due to image paths not consistently working. To this day I still have to deal with an issue where links to other pages sometimes break by including random backslashes and incorrect paths in the final site regardless how the path is written, but after several years of just using it for what I need it to, learning to use plugins for certain features like video, and finding a consistent flow, the whole process definitely became easier. Now I can just write my notes as I would normally, with a little bit of extra formatting, like using headings to organise a page or using admonitions as a visual way to "embed" other pages' content.

??? info "Click Me"
    Here's an example of an admonition...


\??? info "Title"
\	And here's how the plaintext looks!

Overall the whole process was a learning experience, but I wouldn't consider it any more difficult than troubleshooting or bugfixing in a small game demo. The biggest stumbling blocks were all just programs being a little bit broken and needing workarounds, which is perfectly doable and something everyone has to learn. Adding content to the site is easy, of course, adding features like search and light/dark modes was extremely easy thanks to MKDocs plugins and since it is just a site displaying my work and not advertising me as a web developer, it's no issue in my opinion that the aesthetic design of the site is just adding extra CSS to Material for MKdocs. I'm happy with the final product, and all my improvements from here (like adding a page for my art projects, fixing file hosting to not rely on Google, or reworking the theme once I'm no longer focusing on just getting the work done) are just going to make up that final 20% to improve it further.

