# FPS Game Template: Technical Specification
## Prepared by: Kiyah Domogala 1162026, Reuban Lindley 1202780, Samuel Romagnoli 1196271
## Class Code: SGD213
## Task Selected: Game Two

# Premise of Assigned Task:
For this task, we were assigned the following:

“Greetings, I’m Daniell, an ex-dev of Bungie from Halo 2. I’m looking for a FPS template that I can use for my classes. I’m tight on time and need to offload this to another team. Looking for a standard Doom, Halo 1-2, Quake, or Duke Nukem-style FPS. Need some basic enemy AI that walks around and looks at the player when in range (They don’t have to shoot back). The player character needs to have 2 guns they can change, a grenade, and movement.”

We have assigned the following tasks among the members:
Kiyah - Player (Player Movement, Player Camera, Health, Pickups)
Rueban - UI and Weaponry (First Person Info, HUD (Health, Amo), Menus, X2 Guns, X1 Grenade)
Samuel - Enemy (2D Sprite, Look at Player, Takes Damage)

# Player Character
# _Player Movement_
The system for moving the players will be created with Unity creating a player movement script with a 3D rigidbody component to create movement using keys WASD. The movement will have responsive and smooth FPS-like controls where the player can use keyboard inputs to move around the world. 
Friction and drag will be altered to ensure that the movements are fluid and natural. Having Collider Component, the players can interact with the environment objects (pickups) such as guns and grenades. Mechanics like sprinting and jumping could be added based on needs of the game, however for the base template, only regular walking will be added in.

# _Player Camera_
The player camera will be a child of the character while maintaining a constant first-person view that is suitable for a 3D FPS game environment requested by the client. The player’s movements will be made using mouse controls to aim the camera, which will help with targeting the enemy more accurately. The player camera will have the vertical axis clamped at a set amount so the player can’t look too far down or up (stopping the camera from doing a full 360 degrees spin vertically).

# _Player Health_
The health script will use a reusable base script shared between the player and enemy. The health will be divided into two parts, Health.x will be the maximum health given to the character, and Health.y being the current health that the character has. All characters start the game with their health at the maximum. When the player or enemy deals damage from projectiles or attacks, the script will subtract the corresponding damage value from the current health variable which will show in the HUD. 
_Health.x - max health, Health.y - current health_

# _Pickups_
The pickup script will allow the player to collect weapons and equipment placed throughout the game world. Pickup objects will use collision detection which will detect the player interacting by walking through the item. Once a collision is detected, the pickup game object will be removed from the scene and the corresponding item will be added to the player’s inventory (displayed in the player’s viewport) for use. Our project will include three main pickup variants: two firearm types and one grenade type as per client’s request. Each pickup object will use the same base script, but also have its own prefab and inventory reference to correctly identify, and equip the selected item during gameplay. The items will be displayed as floating 2D sprites in the scene using simple designs to distinguish them from one another.

# User Interfaces
UI for video games are very important, they provide an overlay for the limitation of the lack of stimuli the player is given compared to reality. For a first person shooter it is especially important for knowing health and ammunition count. Due to the fast paced nature of these types of games, knowing these things on the fly are crucial.

# _Basic Menus_
While this was never asked for, I feel it is a base requirement for debugging and development of any project. A main menu, pause menu, and a settings menu can be very convenient in developing an FPS, and in general for a more finalised product. These are not very complex, simply a single script where each button in Unity activates a part within, allowing level changes, debugging information settings, and to even pause the game.

# _Health and Ammunition_
This can be one of the most important parts of an FPS, and typically are quite simple. Both of these values are going to be represented as both bars, and numbers for this project. The health float, in this case Health.y is going to be represented as a number going from 0-100, and next to it a bar that shows the same read as text. At least for the number here, depending on its value it will change its colour, depending on the value, below 20 red, below 30 yellow, and above that it’s default colouring.

The magazines Vector2 is the same as bullets; they are broken down into two parts, for the magazines X part being the number of magazines the player has and the Y part being the number of bullets in the current magazines. Now given this single variable has two parts it’s going to be broken down into the UI. The number on the side will represent the Ammo.X or the amount of magazines, and the bar represents Ammo.Y or the amount of bullets in that magazine. 

# Weapon Selection
While this wasn’t directly requested, having a way to select what weapon the player is using is very important when it comes to an FPS, and also debugging and game development. The idea is quite simple: have two boxes, the top one showing there is a gun, the bottom showing how many guns you have in that category, and then a number on the top box showing what button to press. Having more than one per category across the screen, if you have two of the same category of weapon you press the number key more then ones and it circles though that category of weapons.

Given that there are three weapons that were requested, by default there will be three categories. However, given that we do not know the extent of the project that this will be a part of, it’s good to add flexibility to the code. Thus, depending on the amount of guns the developers decide to add the code will be designed to accommodate it.
To break it down it’s a set of gameobject and other arrays inside the stats script, WeaponKeyNUMS, GunButtonKey, and WeaponBoxes. Each of these arrays are set to the GunNumber.Length, the GunNumber array is an int value, which is set via another script, to whatever number guns the developers will need or want. These are just the main arrays for the simple visual functionality.
The main arrays for the visual functionality are;

_WeaponBoxes: a Gameobject that shows the number of weapons
WeaponKeyNUMS: a Gameobject array for the Number in the canvas
GunButtonkey: a TextMeshProUGUI array to edit the Numbers_

The code will instantiate the gameobject into the gameobject arrays in a for loop. This will be done both for the WeaponBoxes and WeaponKeysNUMS, offsetting them by WBNP and WKNP Vector3’s multiplied by ‘i’ respectively; they are also set as the children of a Gameobject in the canvas so they are visible. 

After, the code should set the GunButtonkey[i] to WeaponKeysNUMS[i] GetCompoent TextMeshProGUI, this allows use to edit the text in the WeaponKeysNUMS gameobjects with GunButtonkey Array, and simply making it equal i + 1, we get the numbers increasing respectively across the screen by the amount of guns we will have. Button presses, simply depending on the amount of guns the number of buttons used in weapon selection can vary, using the Input.GetKeyDown, we can add to a value to know what gun in what category is selected.
Guns and Weapons
The guns in this game are controlled by two scripts, you have the main weapons script that controls the amount of guns, their damage, fire speed, and number of magazines and size of the magazines.
You can set the amount of guns and each gun’s settings, this allows you to add any number of guns and for quick prototyping and implementation. Given we do not know this game's aesthetics exactly, as we do not know if the guns are going to be represented as 2D sprites or 3D assets, the implementation of animations and audio will be quite simple to be put into the hands of the main development team. All weapons are going to be pickups, if the player has no weapons, the weapons selection will still show the max amount of them, just without the weapon identifier boxes below each category. If the player picks up a weapon they already have it will be added to the ammunition, the of that weapon rather than replacing the gun the player is currently using.


The grenade will be a gun preset in the creation of the weapons a bool is_throwable, can be checked, and thus throwing past the ammunition count, will remove the gun from your weapon selection as they are consumed. Grenades, are quite simple, have a held throw, instantiate the prefab gamobject it having a timer, have a ridged body capsule or cylinder mesh, after delay have it throw rays in each direction, depending on the distance that ray travels proportional to damage, then delete itself.

# Enemy
Enemies will consist of empty game objects with capsule collider components, and a plane game object as its child. The collider components will function as the hitbox and will be responsible for determining the direction the enemy is facing, while the plane game object will display the 2D sprite and always be facing the player. These game objects and components will be managed by 3 – possibly 4 – scripts, one for walking around (idle behaviour), the 2nd for looking at the player (enemy engaged), the 3rd to manage enemy health, and maybe a 4th script for enemy weapons. While idle, enemies will be walking and looking around, but won’t stray too far from their spawn location, and while engaged, enemies will stand still and look at the player until either the enemy dies, or the player walks out of range/out of sight, in which then the enemy will become idle once again.

# _2D Sprite_
The enemy sprite will be displayed on the plane gameobject that will be constantly facing the player. The plane gameobject will have its up and down rotation locked to ensure it can only turn around left and right. This is just in case the player jumps, the plane’s rotation doesn’t rotate upwards to face the player.

# _Look at Player_ 
While the plane game object will continuously be facing the player no matter what, the enemy itself can face other directions, as determined by the empty game object with the capsule collider. There are 2 ways in which the enemy could spot the player. The first would be to make the enemy immediately engage in combat with the player when the player gets close enough, and the second way would be to use raycasting to see if the player enters the enemy’s line of sight. The latter would be more realistic, but harder to implement, which is why the first option might be best suited for this kind of game, as this game is supposed to be more action packed, rather than just letting the player sneak behind enemies.

# _Take Damage_
The player health script is going to be used for enemies as they function the same. The maximum health of an enemy (Health.x) can be configured in the inspector for different enemy types. Enemies will take damage from any source just like the player, but maybe making enemies only take damage from player damage sources could help prevent the enemies from killing each other.
