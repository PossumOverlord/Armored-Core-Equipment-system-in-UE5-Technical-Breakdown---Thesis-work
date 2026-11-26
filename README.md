1.
My name is Teoman Bayindir and this is my thesis work for Yrgo’s game programmer course. This work was done midst the development of the game [PROJECT SPELLRUSH] and this thesis work will describe how to create an equipment system in the vein of Armored Core VI Fires of Rubicon in Unreal Engine 5. AC6’s equipment system lets the player customize parts of their mech. Which leads to players feeling a sense of ownership of their character, expressing the way they want to tackle the game's various challenges, and getting a sense of satisfaction upon beating said challenges with their own brain child. When one faces failure, it encourages having to take a step back and analyze, then strategize about what equipment could be of more use. This is why I deem this system worthy of replicating.

[PROJECT SPELLRUSH] combat is (as previously mentioned) inspired by Armored Core VI Fires of Rubicon’s combat systems, but shifts its focus entirely on 1v1 multiplayer combat. As two witches partake in a rite where only one may survive. 


2.

My thesis question is:  “How do you create an equipment system in Unreal Engine 5, in the vein of Armored Core VI Fires of Rubicon. What my work boils down to is how we can go about using UE5’s tool set to design a system that captures the essence of AC6’s equipment system, rather than mimicking it exactly, as AC6’s system is quite expansive and had a lot of content packed into it. Are there systems that can be abstracted? As in can certain parts of the system be scoped down, but still have similar strengths to the original? Also how will the system need to change to reflect the needs for this particular game? Additionally how will developing this as part of a team change how it's developed? [PROJECT SPELLRUSH] is heavily inspired by Armored Core VI in most aspects of its design, but there will be key differences from the original, as the game tries to find its own identity. 



3. Areas of Research
Armored Core VI
How does AC6 encourage the player to express themselves and engage with its various systems? Variance in level structure and what enemies you are facing makes the player 
How would we store static data?
In order to create equippable spells for this game an approach that would make it so other team members could easily create spells on their own would be required, as is good practice when working with other developers.

Blueprint Function Library 
A Blueprint Function Library most common use case is to create functions accessible on a global scope, helping you avoid placing the same nodes over and over again. Although a BPFL can not contain any variables accessible on a global scope, its functions can instead output variables hard coded into its functions.

Pros:
Easy to reach via blueprint.

Cons:
Very unintuitive to use for non programmers that are part of the team.
Finicky to change, as it's layered under a lot of Unreal’s UI.
Blueprint function libraries have no inheritance, which demands a lot of hard coding when implementing them and more gruntwork when creating new stored spell information.
Data Assets
You can imagine Data Assets as an ordinary UE object with exposed variables, you define what set of variables types the object will contain (in a Primary Data Asset), then are able to create assets where you set the values of said variables. Primary Data Assets can also inherit from other PDAs. If you are familiar with Unity engine’s Scriptable Objects,  

Pros:
Can inherit. Cuts down on gruntwork. 
Is an asset. Easy for everyone if they just want to change a number of a spell, or create an entirely new one.

Cons:
Requires a reference. Making it not as easy to acces as a BPFL. Although this lets you slot in whatever Data Asset one wants, so it is a drawback that comes with many benefits.

A common theme when researching things for UE5 will be that their documentation does not help you very much. In this case most of this information is easiest to find through Youtube tutorials and user made forums.


How would we store player inputted data at runtime?
UE5’s primary way of saving data is through the blueprint class Save Game Object. The Save Game Object is like most other objects, the key difference being that it is saved in memory, and therefore it can store its data between levels and between sessions of playing the game.
How is game UI created in UE5 and what is best practice?
Widgets
UI is an integral part of the system we intend to recreate, as it is the main way the player interacts with it, we need to delve into what common practices are in UE5.
UE5’s widgets is a tool that lets you design a User Interface, by placing individual pieces of UI called “Widgets” in a hierarchy, where they can be parented and childed to one another. There are 40+ widgets to choose from and in order to understand what each individual widget does there are a good amount of tutorials online that explain best practice. 
What most tutorials showcase is that most UI elements consist of many widgets combined to fulfill one specific use case, a button that has a label on it, it uses the Button widget (which allows it to be clickable) and the Text widget (which displays text). 
Some common shortcomings many warn sources warn you of is that changes to a custom widgets structure can be troublesome as a significant amount widgets can only have one child. This will be showcased later as it was something that came up quite often. Additionally another pain point of widgets is that when one widget is nested in another user made widget, it can then only be changed in that user made widgets blueprint. Variables of nested widgets are completely inaccessible, unless exposed via visual scripting, but this has to be done to every user made widget, which wastes a lot of time but unfortunately this is a vital part of how UE5’s widgets work, although it is not a flaw that warrants looking into alternatives as UE5 as is does not have any other ways of creating UI.

Widget Components
Widget Components are an experimental feature in UE5, objects one can latch onto a widget, not as a child, but as extension of the widget’s functionality. Unfortunately there is no easy way to do this via blueprint and the time it would take to create this via a C++ solution would not be in the scope of this project nor save the amount of time it would take to implement.





Replaying Armored Core VI Fires of Rubicon
AC6’s main way of making the equipment system engaging is through every piece of equipment having a wide variety of different strengths and drawbacks. Weapons that are hard hitting will have to sacrifice an attribute like weight and or rate of fire. Even equipment like what kinds of leds you use matter as ones providing better defences, may come at the cost of slower move speed. Keeping in mind that everything must come at a cost is important when making adjustments to this system.

Things I have researched:

Widgets
Widget components
Interfaces
How inheritance works in UE
Replayed Armored Core VI
Best format for devs to create equipable things
Save Objects


4. The Process
Creation Data Assets For Spells
First thing to implement were the Data Assets for spells, as to not bottleneck other team members that depend on. 

First interaction of spell Data Assets

This first iteration of the DAs were easy for the other team members to use. The heritance tree all growing from PDA_SpellDefintion intended to make it clear what type of spell one would create, whilst it did partially achieve this the team requested that there should be fewer spells that and that were more generic as the inheritance of the data assets was a little unintuitive to follow.
Second iteration of spell Data Assets

Simplifying the system down to 3 data assets, two of which inherit from PDA_SpellDefinition ended up being a lot easier to use for the others. Many of the optional properties of a spell are now turned on by setting a bool to true or changing the value of an enum.
Creation of the games save system

In UE5 saving data is achieved via the “Save Game to Slot” function.










In order to avoid keeping track of the string of each piece of stored information, a custom save and load function was required as each class of save game object could store a unique string.

The load function is not as easy to understand as the save function, but in short you input the class of the save you wish to reach and it returns its corresponding save object.
This was also made intentionally generic enough for other programmers to not just save things related to the equipment systems. 

Creation of UI and its Widgets
Base class for buttons

The base button consists of the button widget and its childed text widget as its essentials. As research had warned these two widgets are hidden when the base button is placed in another widget blueprint:
 
The process of creating buttons and revisiting them later as new functionality was added, shows just how much of a time sink it can be to expose the variables of the base button’s widgets. Here is a quick demonstration:

Here we can see the button’s color and opacity variable and it can easily be changed in the base buttons blueprint. But since the base button’s children are hidden when placed inside another blueprint we can not reach this menu, we cannot change the color of each individual base button. To circumvent this we make a public variable called “color”. Now this color variable is not the same as what we want to reach but we can set the color we want to reach by code. This sets the color of the button to the same color as the public variable.

Another problem with button base class was also found during the process. To demonstrate this, let's say we have created a base class button without text and a child button that inherits from the base class.
We then want two more buttons. One without text that calls an event when hovered, another with text that calls an event when hovered. In total we seek to create for different buttons, the inheritance of the first two is simple, the other two would require that they inherit from the other two then implement the exact same hover functionality twice. This might not sound like a lot but if every optional functionality must be applied to more than one button, it can quickly add up and become a lot of gruntwork, if one wants to tweak functionality it would have to be manually done to every button where it is implemented. This could have been fixed if one could attach widget components with the desired functionality to the buttons, but unreal does not support user made widget components. Even though Actor and Scene objects can have custom components.

This informed the choice of packing the base button with as much functionality as necessary, its tradeoff being its cluttered variables.

If a variable is left empty, then the code ignores the variable's corresponding functionality.

Problems when refactoring widgets
Just as research had warned, widgets are very strict with their structure and this was definitely felt when refactoring. As it took time for someone on the team to produce background images for all the buttons in the game, background image functionality was not supported in buttons yet, and when it came to implementing the image it required a restructuring of the base button class as its button widget could only contain one child, it required a lot of experimentation as it is not very obvious what widget could solve this issue as the “border” widget that has a background image variable always stretches the image to fit the border’s size, which was undesired.

The overlay widget wound up being the solution to the problem as it gets around the one child limit of the button and can overlay as many of its children as possible. 

Equipment selection UI and Equipment System Design Philosophy 
We decide as a team to simplify AC6’s game design for customization, by reducing the type of different things you could equip down to five 5 (down from twelve), this was done to not only minimize the amount of content we would have to produce but also but our own spin on the system by introducing the Weight system. As we decided, the Weight system lets you, whenever you choose a spell, also choose that spells “weight”, that weight is represented visually on your character corresponding to where it is equipped and grants that spell bonus effects like improved damage or increased projectile amount, but comes with the cost making your character way more slowing them travel slower. Although this is a departure from how AC6 works, it still maintains the principle of the players choices coming with a cost and it's a way of making it feel as though our limited amount of spells we can create as an indie time feel like there are a lot more of them, as Weight can alter a lot of things.

In order to store spells that should be in rotation for the player to use. Another Data Asset was created, a BPFL could have also achieved this, but in this case an asset that could be assigned as a variable was more convenient as it would not require hard coding of where to find these spells. 


This solution allows us to insert any SpellsInRotation Data Asset in the used spells variable, which then generates clickable buttons for each spell we have decided the player can use. This is very easy for other developers in the project to use as they can remove or add spells to the data asset without touching anything else. 

As seen here in the SpellClickable button’s variables, we can assign what spell saver for the button to use. The spell saver is a class that inherits from our custom Save Object, and makes saving what the player clicks on much simpler.
The BP_SpellSaver class:


5.

6.
