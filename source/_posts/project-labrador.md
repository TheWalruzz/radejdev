---
title: Project Labrador - Data Persistence
date: 2020-05-21 07:13:10
tags: 
- project-labrador
- gamedev
---
Hi folks, since I've decided to take a short break from developing **Project Labrador** (at times I already felt burnt out, so it's better to relax for a bit and rethink stuff all over again), it's a perfect opportunity to tell you a little bit about the project.

Like I mentioned in the {% post_link introduction Introduction %}, project was born on **July 26th 2019**. I was at work, waiting for a new project (which came just days after) and began to think: if Unity is scene-based, with no central data storage or manager, how do you keep track of entities and their data between scenes and also how to make it easy to save to a file and load it later? The answer came to me after a short research. Since I never did or had to implement such a system, I started to work on a prototype. I was excited. I finished it in 2 or 3 days.

Here's how it works (please note that this is just my naive approach that seems to be working fine in my case. It has it's quirks and pitfalls that you have to be aware of!):

* Attach Entity component to the GameObject that you want to keep track of and make sure to set a proper id (generate a new one for new entities or copy id used before).
* When the scene is loaded and the Entity Start method is executed, it syncs itself with the GameManager that stores all the entity data (inventory, stats, attributes etc.) that I wanted to be saveable. *(NOTE: all the data that won't change between scenes (like looks etc.) is not saved to the file. This way it's possible to create NPCs that look differently based on some conditions in the game.)*
  * If the data for said Entity are already stored in the GameManager, the Entity component's internal data changes to the one in the GameManager.
  * If data does not exist in the GameManager, component's data is copied there and stored for the future.
* If data is stored in the serializable State object in the GameManager, it can be serialized and deserialized with one method.

And that's it. Incremental entity data storage. This means that each entity component is filled in editor with its **default** values, allowing me to make a prefab of said entity to use in other scenes and ensures that even if we skip the scene where the entity was supposed to be first encountered, data will initialize properly and all the changes to his state will be persisted. Also, after some tweaking I was able to add something I called temporary entity - entity generated in runtime - at the moment it's only used for items dropped from inventory, but in the future it will be possible to create some randomly generated enemies and so on.

Of course, this limits some options when it comes to creating scenes: entity instances live in a specific scene and cannot just leave it. They're basically stuck, unless I simulate them going into other location and create an entity with their id in the other scene. For this purpose I had to create utility components that show/hide entities based on some conditions. Pretty hacky and limiting, but it should be okay for my purposes. I don't expect to be doing that kind of stuff often anyway. Just keep it simple and it'll work. I hope so at least.

Okay, that's about it. This is just a tip of the iceberg that is my RPG framework. Let's just hope I won't crash into it in the long run 😉. Bye for now!
