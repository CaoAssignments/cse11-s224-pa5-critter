# CSE 11 Summer Session I 2024 PA5 - Critter and Inheritance

**Due date: Sunday, August 4 @11:59PM PDT**

**Download the starter code from GitHub by clicking: Code $\rightarrow$ Download ZIP**

## Provided Files

The following files are provided:

* `Critter.java`: prototype of a critter object, contains a class of Critter and enums Direction and Attack.
* `Starfish.java`: Patrick Star, he lives in Bikini Bottom.
* `Feline.java`: starter code for Feline.
* `CritterInfo.java`: provides context about the Critter and the current simulation. (**Please read the method headers provided to gain a better understanding.**)
* `CritterGUI.jar`: A GUI simulator for your classes. It contains `Raccoon.class`, which is a mysterious `Critter` with unknown behavior.

**Note:** the starter codes do not compile by doing `javac *.java` because they are not completed. When you complete a class, you should write a tester to test your individual methods in that class. In the end, you can use `CritterGUI.jar` to run the simulation. 

## Goal

This assignment is an introduction to inheritance in Java. In this assignment, you will write several classes for various critters who will face off against each other in an arena.

## Overview

Critter [Gradescope, 100 points]

- Implementation [95 points]
- CustomCritter [5 points]

## Arena & Simulation Mechanics

Several classes in the starter code implement a graphical simulation of a 2D world with many animals moving around in it. You will write a set of classes that define the behavior of these animals. For each class, you are defining the unique behaviors for each animal. 

The Critter World is divided into cells with integer coordinates. The world is 60 cells wide and 50 cells tall by default. The upper-left cell has x-coordinates and y-coordinates (0, 0). The x-coordinate **increases to the right**. The y-coordinate **increases downwards**. 

![](https://github.com/CaoAssignments/cse11-s223-pa5-critter-starter/assets/114690912/f35f36da-e215-4bbb-af6e-dad7548609b5)

**NOTE: the following mechanics are already implemented in the Critter simulator, e.g. when two critters fight, the simulator decides who wins based on what the attack each critter uses. Your task for this assignment is to define behaviors for each critter. i.e. you just need to write the methods but don't need to call them.**

You can run the simulator to test your critter implementations.

### Movement

When it’s Critter A’s turn to move, the simulator will call `A.getMove()` to get a direction.

Each round, each Critter can move one square north, south, east, west, OR stay at its current location (i.e. center). The world has a finite size, but it wraps around in all four directions (for example, moving east from the right edge brings you back to the left edge).

### Fighting

As the simulation runs, animals may collide by moving onto the same location. When two animals collide and they are from different species, they fight. The simulator will call `getAttack()` so that the critter fighting can select which attack to use.

The winning animal survives and the losing animal is removed from the game. Each animal chooses one of `Attack.ROAR`, `Attack.POUNCE`, `Attack.SCRATCH`, or `Attack.FORFEIT` as their attack mode. Each attack will have some other attack that it is stronger against (e.g. Roar beats Scratch). If an animal decides to Attack.FORFEIT, it will automatically lose the fight.

The following table summarizes the choices and which animal will survive in each case. To remember which beats which, notice that the starting letters of "Roar, Pounce, Scratch" match those of "Rock, Paper, Scissors." If the animals make the same choice, the winner is chosen with a coin flip.

![](https://github.com/CaoAssignments/cse11-s223-pa5-critter-starter/assets/114690912/2239aa25-085b-4e6f-8f9d-3f0be4fa75d0)

The cases above are based on the following rules:

1. Critter #1 scratches and Critter #2 roars. Critter #2 survives.
1. Critter #1 roars and Critter #2 pounces. Critter #2 survives.
1. Critter #1 pounces and Critter #2 scratches. Critter #2 survives.
1. Critter #1 and Critter #2 use a same attack in [roar, pounce, scratch, forfeit]. One of them survives, chosen randomly.
1. Critter #1 uses an attack in [roar, pounce, scratch] while Critter #2 forfeits, Critter 1 survives.

### Mating

If two animals of the same species collide, they "mate" to produce a baby. Animals are vulnerable to being attacked while mating, so any other animal that collides with them will defeat them both. An animal can mate only once during its lifetime. A “baby” will be spawned as a full adult next to its parents once they are finished mating.

The mating behavior is enforced by the simulator. However, when two critters mate, `mate()` will be called so that your critter knows it starts mating.

### Eating and Sleep

The simulation world also contains food (represented by the period character ``"."``) for the animals to eat. There are pieces of food on the world initially, and new food slowly grows into the world over time. As an animal moves, it may encounter food, in which case the simulator will ask your animal whether it wants to eat it. Different kinds of animals have different eating behavior: some always eat, and others only eat under certain conditions. Once an animal has eaten a few pieces of food, that animal will be put to "sleep" by the simulator for a small amount of time. During the sleeping period, the animal will automatically forfeit all fights, meaning it will lose to all other critters that attack it.

When a critter encounters food, the simulator will call `eat()` to ask whether the critter wants to eat or not.

The sleep behavior is enforced by the simulator. However, when a critter sleeps, `sleep()` will be called so that your critter knows it starts sleeping. Similarly, `wakeup()` will be called when your critter finishes its nap.

### Scoring

The simulator keeps a score for each class of animal, shown on the right side of the screen. A class's score is based on how many animals of that class are alive, how much food they have eaten, and how many other animals they have defeated. Detailed formula below:

```
Score = 3 x Survivor + 3 x Killed_Opponents + 1 x Food_Eaten
```

## Implementation Details

Each class you write in this section will inherit from a superclass (`extends`)  and may be inherited by a subclass. We take advantage of inheritance in two ways: 

* Since subclasses automatically inherit certain methods from their superclass, if we want a certain method to be uniform across a family of classes, we can simply define the method in the superclass. 
* The other advantage is that we can minimize the amount of code that we have to write, which reduces the possibility of errors. Inheritance provides the programmer assistance in streamlining the code writing process.

Below is a diagram for the Critter World. Critter is an abstract class and you can read more about abstract classes [here](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html).  All the other critters/animals are implemented as **concrete classes** (meaning they can be instantiated by using the `new` keyword).

Each class must extend Critter (or something else that extends Critter). 

![](https://github.com/CaoAssignments/cse11-s223-pa5-critter-starter/assets/114690912/6c6530db-df3a-4da1-a1c8-b22e1c44ef2c)

### Running the Simulator

Note: Don't try doing this before you have implemented the files. It will not even compile.

We will now go through the process of running the simulator. First compile all the necessary classes (note: `Starfish.java` and `Feline.java` as it is provided will not compile). You can compile separately if needed, but `javac *.java` is most convenient. Next, run the simulator with `java -jar CritterGUI.jar`. 

The following screen will appear. Enabling “Debug output” will print the actions your critters take to the terminal. You can also choose the number of critters that initially appear in the world, and select which critters that will start in the world.

![](https://github.com/CaoAssignments/cse11-s223-pa5-critter-starter/assets/114690912/cb2a478e-e9ea-4b98-96b0-c52dd053343b)

You have many options in the next screen. You can start, stop, and adjust the speed of the simulation. “Speed” controls how fast each round occurs. For visual testing, you can click Tick, which will run one round of the simulation. If you would like to toggle debug messages on your terminal as your simulation runs, click the checkbox for “Debug”. 

![](https://github.com/CaoAssignments/cse11-s223-pa5-critter-starter/assets/114690912/2e8b1513-a33b-4de7-8e60-04257f6fa7ac)

### Implementation

> **Tips:**
> 1. Read and compile Critter.java before anything else!
> 2. You are **only allowed** to import these libraries:
>    - `java.awt.Color`
>    - `java.util.Random`
>    - `java.util.Arrays`
>    - `java.util.ArrayList`
> 3. Read through all the methods you are supposed to implement for a class before starting to write anything for that class. These methods are related to each other most of the time!
> 4. **Don’t use any methods in CritterInfo in the constructor.** They’ll be set after the objects are initialized and loaded into the simulator, so using that in the constructor will create NullPointerException. 
> 5. For the overriding methods (specified below), **please use `@Override` annotation** to ensure best practice. This annotation indicates that the child class method is over-writing its super class method. If this annotation causes a warning, that means your method is not overriding, and could indicate issues with your method signature.
> 6. **Study the diagram provided in the earlier section carefully**. The Critter World has animals where each animal is of type **Critter**. The animal can either be of moving type (i.e. Leopard) or stationary type (i.e. Starfish). When color is involved, use the static colors that comes from the [Color](https://docs.oracle.com/javase/10/docs/api/java/awt/Color.html) class that have the same name as the color we specify. 
> 7. Other than `Starfish.java` and `Feline.java`, you will **need to create each of the other critters' .java files from scratch**.

### Critter 

This is an abstract class defining all the possible methods that can be called on a Critter. **Do not modify this class**. You may need to override some of these methods in the other classes.

Study this class to get to know all the methods you can call, and all the instance variables you may access.

### Starfish extends Critter

Starfish are very interesting creatures. One particularly interesting specimen is the lovable goof and best friend of SpongeBob Squarepants, Patrick Star. Patrick will be inhabiting our arena as a representative of the starfish. 

* **No-arg constructor**: The string representation of Starfish is “Patrick”.
* **Do not override `eat()`**: Patrick is not interested in eating, so there is no need to override `eat()`.
* **Do not override `getAttack()`**: Since Spongebob isn’t around, Patrick is feeling pretty chill so Starfish inherits the default Critter behavior. When Patrick is attacked, he will FORFEIT, so no need to override the `getAttack()` method.
* **Override `getMove()`**: Unfortunately since Spongebob isn’t around in the arena and the fact that Starfish are inherently pretty motionless creatures, there isn’t much Patrick wants to do. He decides to stick around under his rock (CENTER). **(A.K.A. It doesn't move.)**
* **Override `getColor()`**: Patrick is PINK (already implemented).

### Turtle extends Critter

* **No-arg constructor**: The string representation of Turtle is “Tu”.
* **Override `getColor()`**: Turtles are GREEN.
* **Override `getMove()`**: Turtles always move WEST.
* **Override `eat()`**: Turtles like to play it safe when eating. They only eat when there are no hostile animals adjacent to the Turtle (hostile animals are anything that is not an empty space, food, or Turtle).
* **Override `getAttack()`**: Turtles don’t always fight, but sometimes they do. Turtles attack with ROAR 50% of the time and FORFEIT the other 50%. Slow and steady wins the race, after all.

### Feline extends Critter

* **Instance variables**: Each Feline will keep track of the number of times it has moved, the amount of times it has not eaten, and the current direction it is going in. These have been provided for you.
* **No-arg constructor**: A default no-arg constructor. The Critter should not be hungry until its third encounter with food. `moveCount` should be set such that it will change to a new direction at the next (first) call to `getMove()`. This means that Feline should not start eating once it is created (look at `eat()`). The string representation of Feline is “Fe”. 
* **Override `getMove()`**: Felines are jumpy and tend to go in random directions, so it will go in a new random direction (excluding CENTER) after every 5th move that it makes. 
    * If the Feline picks NORTH as its first direction, and SOUTH as its next random direction its moves would be: NORTH, NORTH, NORTH, NORTH, NORTH, SOUTH, SOUTH, SOUTH, SOUTH, SOUTH.
    * NOTE: "new random direction" here means a newly chosen direction, not necessarily a unique/different direction. If Feline went NORTH for 5 moves, it should still be able to choose NORTH as its next 5 moves.
* **Override `eat()`**: Felines don’t need to eat that much, so every 3rd time it encounters food, it will eat.
    * NOTE: The feline will eat after 2 times of encountering food and not eating. For the first 6 times the feline encounters food, its eating pattern should be:
    false, false, **true**, false, false, **true**
* **Override `getAttack()`**: Felines should always POUNCE.

### Lion extends Feline

* **Instance variables**: Each Lion will keep track of the number of fights it wins until it goes to sleep, which will determine its eating behavior. Losing a fight does not decrease the instance variable. You may need a few private variables to keep track of the Lion’s movement.
* **No-arg constructor**: A default no-arg constructor. The string representation of Lion is “Lion”. Changing the name after calling the no-arg constructor is preferred.
* **Override `getColor()`**: a Lion is YELLOW. 
* **Override `getMove()`**: The king of beasts does not fear other critters, and they wait for their chance to engage. A Lion will first go EAST 5 times, then SOUTH 5 times, then WEST 5 times, then NORTH 5 times (i.e. a clockwise square pattern). Think about what instance variable to initialize to keep track of its movement.
* **Override `eat()`**: Return true if the Lion has won _at least one fight_ since it last ate or slept. Think of the Lion as having a “hunger” that is triggered by fighting. Initially the Lion is not hungry, but winning a fight makes the Lion hungry. When a Lion is hungry, the next call to the eating method should return true. However, once the Lion has eaten OR slept (when `sleep()` is called), the future calls of `eat()` should return false until the next win. (Hint: Think about how `sleep()` would make a lion full again.)
* **Override `sleep()` and `wakeup()`**: When a Lion goes to sleep, it will reset the number of fights it won to zero, and reverse its display name to “noiL”. When it wakes up, it reverts back to “Lion”. 
    * Note: Handle the behavior of `sleep()` and `wakeup()` separately in their own respective methods.
* **Override `win()`**: When a Lion wins a fight, it becomes hungry. Make sure to keep track of the number of fights it wins as this affects its eating behavior. 
* **Do not override `getAttack()`**. It should have the same behavior as Feline.  

### Leopard extends Feline

* **Variables**: Each Leopard, in addition to the instance variables inherited from its superclasses, will all telepathically keep track of their `confidence` together. The confidence starts at 0 when the simulation starts. When the confidence of one Leopard is affected, ALL Leopards' confidence will be affected in the exact same way.  
    * **Hint:** What type of modifier can you apply to a variable to make that variable shared across all instances?
    * Make this variable `protected`. The name should be `confidence` and the type should be `int`.
* **No-arg constructor**: The string representation of Leopard is "Lpd".
* **Override `getColor()`**: Leopards are RED (for camouflage, of course).
* **Override `getMove()`**: The Leopard always checks its neighbors before moving. If one of the four neighbors—NORTH, SOUTH, EAST, WEST— contains either food or Starfish, then the Leopard will move towards that direction. If more than one direction has Starfish or food, then the Leopard will move towards the first found direction. If none of the directions contain Starfish or food, then the Leopard will randomly choose a direction to move (excluding CENTER). (When checking the neighbors, please follow the **N S E W** order).
* **Override `eat()`**: The Leopards will always have (confidence * 10)% chance of eating. For example, if confidence is at 2, then there is a 20% chance of eating.
* **Override `win()` and `lose()`**: If a Leopard wins a fight, all Leopards’ confidence will increment by one if their confidence is less than 10. If a Leopard loses, all Leopards will reduce their confidence by 1 if their confidence is greater than zero. The minimum confidence they can have is 0, and the maximum is 10.  
* **Override `getAttack(String opponent)`**: The Leopard will POUNCE if the opponent is Turtle or if all Leopards' confidence is greater than 5. Otherwise, the Leopard will randomly choose an attack method using `generateAttack(String opponent)`.
    * You **must** create the **protected** helper method `generateAttack(String opponent)` to randomly choose an attack method between POUNCE, SCRATCH, and ROAR. However, if the opponent is a Starfish, it will FORFEIT.
* **Override `reset()`:** Set the `confidence` back to 0.

### Ocelot extends Leopard

* **No-arg constructor**: The string representation of Ocelot is "Oce".
* **Override `getColor()`**: Ocelots are LIGHT GRAY.
* **Override `generateAttack(String opponent)`**: If the opponent is a Lion, Feline, or a Leopard, the Ocelot will SCRATCH. Otherwise, the Ocelot will POUNCE. **NOTE: This method is called from getAttack(String opponent)**
* **Do not override `getAttack(String opponent)`**: The Ocelot will attack in the following ways:
    * If confidence is greater than 5 or the opponent is a Turtle, the Ocelot will POUNCE
    * Otherwise, if the opponent is a Lion, Feline, or a Leopard, the Ocelot will SCRATCH. If not, the Ocelot will POUNCE. **NOTE: This takes place in `generateAttack(String opponent)`**

### Elephant extends Critter

* **Variables**: Elephants share an int `goalX` and int `goalY` coordinate (make them protected, not private). An Elephant also inherits a `Random` object (from `Critter`) to generate random integers when picking new goal coordinates.
    * **Hint**: For elephants to share coordinates, what should be included in the modifiers of `goalX` and `goalY`?
* **No-arg constructor**: The string representation of Elephant is “El”. The very first goalX and goalY should be (0,0).
* **Override `getColor()`**: Elephants are GRAY.
* **Override `getMove()`**: Elephants are sensible creatures and know that they are safer if they move in herds. Because of this, Elephants have a precise movement pattern.
    * All elephants move towards the shared `goalX` and `goalY` coordinate in the simulation. Each Elephant moves towards their goal in the axis in which they are **further from their goal**. So if an Elephant is further from its goal in the x-axis, it would move EAST or WEST depending on the location of their goal and their current location. When an Elephant is  further from its goal in the y-axis, an Elephant would move NORTH or SOUTH. If the distances are **equal**, choose to move on either the x or y-axis (but be consistent with your choice - always choose x or always choose y).
    * If an Elephant reaches the `goalX` and `goalY`, it must change the `goalX` and `goalY` variables to a new random location on the board. Elephants should check for this at the very beginning of their `getMove()` method (and choose random new goal at that time, before choosing their move).
    * **You do not consider the "wrap-around" case for either axis.** For example, consider the case where the world is 60x50, ElephantAlpha is at (0, 0), and the goal is at (59, 5). Then ElephantAlpha will move **EAST** because `goalX` is further than `goalY`, even though it can technically move WEST to get to the `goalX`-coordinate faster on the x-axis. The same holds true for the y-axis. 
* **Override `eat()`**: Out in the wild, Elephants need a lot of food, so they will always eat.
* **Override `mate()`**: When an Elephant mates, increment its level by 2.
* **Do not override `getAttack()` from the Critter class.**
* **Override `reset()`:** Set goalX and goalY should be (0,0)

## CustomCritter [5 Points]

Up until this point you have implemented critters where we guided you through the process. Now it is time to create your own critter from scratch.

**There is no starter code for this class because the implementation is entirely up to you.**

Create a file called `CustomCritter.java` as your own Critter. Your Critter must have at a **minimum**, its:

* own display name (must be appropriate, different from those of other critters)
* color choice (`getColor()`)
* override the `getMove()` and `getAttack()` methods
* inherit from proper class (extends Critter)
* all fields (instance and class variables) are private

You should aim to make it as strong as possible. Be sure to test out your Critter's strength with various critter combinations that you implemented from the previous parts.

We have provided for you a `Raccoon` which will be present in the simulation when running `java -jar CritterGUI.jar`. 

The goal is that your CustomCritter's implementation is able to defeat the Raccoon regularly. You are not given the .java file, so you cannot look at the code to see the Raccoon’s weaknesses. Instead, you should study its behavior in battle to see what steps your critter should take to defeat it.

To offset the difficulty of defeating `Raccoon`, all the scenarios used in the autograder tests are given below. In all scenarios, only `Raccoon` and `CustomCritter` are enabled and put in the world. You can optimize your `CustomCritter` specifically for these scenarios. Nevertheless, you will not be able to see the results of these tests until grades are released.

| Scenario | Width | Height | Number of each critter |
|----------|-------|--------|------------------------|
| #1       | 10    | 10     | 5                      |
| #2       | 30    | 5      | 10                     |
| #3       | 50    | 60     | 25                     |
| #4       | 60    | 70     | 40                     |

## Submission & Grading

Submit the following file to Gradescope by **Sunday, August 4 @ 11:59PM PDT**.

* Starfish.java
* Turtle.java
* Feline.java
* Leopard.java
* Ocelot.java
* Lion.java
* Elephant.java
* CustomCritter.java

**Important:** Even if your code does not pass all the tests, you will still be able to submit your homework to receive partial points for the tests that you passed. **Make sure your code compiles on Gradescope in order to receive partial credit. The file names need to be correct, otherwise no points will be given for the submission.** 

### How your assignment will be evaluated

* **Implementation Correctness** [95 points] You will earn points based on the autograder tests that your code passes. If the autograder tests are not able to run (e.g., your code does not compile or it does not match the specifications in this writeup), you may not earn any credit.
    * **Public Tests** [62.5 points] These are tests of which you can always see the results after submission.
    * **Hidden Tests** [32.5 points] These are tests not visible to you until grades are published after the deadline.
* **CustomCritter** [5 points] **Your own Critter may not win the simulation every time**. Therefore, you need to ensure that your custom Critter performs with enough strength to beat the odds. We run your custom Critter against the Raccoon with different scenarios. The Critter with the highest score at the end of 1000 ticks wins. **Regrade requests for rerunning the simulation will be rejected.** For each scenario, there will be **30** runs of the simulation. Beating the raccoon in at least 10 out of the 30 runs will give you full credit for that scenario.
    * **Public Tests** [0.5 points] These are tests of which you can always see the results after submission.
    * **Hidden Tests** [4.5 points] These are tests not visible to you until grades are published after the deadline.
* The prototypes of the classes in the submission list are correct. Remember that you may create any number of additional helper methods and constants (as fields), but helper methods must be `private`, and constants must be `static final` fields of primitive or String types. Correct prototypes mean:
    * For classes other than `CustomCritter`, the inheritances are correct, there are no other methods besides the required methods above and helper methods, and there are no other fields besides the required fields above and constants.
    * For the `CustomCritter` class, the inheritance is correct, there are a no-arg constructor and methods `getColor`, `getMove`, and `getAttack`, and all fields are `private` or constants.
* You are only allowed to call methods in the classes you have implemented (in the list of submission files above) and library classes listed below, in all your submission classes except `CustomCritter`.
  Excluding in `CustomCritter`, calling methods in other library classes will result in a 0.
  * `Critter`
  * `Critter.Attack`
  * `Critter.Direction`
  * `CritterInfo`
  * `java.lang.Boolean`
  * `java.lang.Byte`
  * `java.lang.Character`
  * `java.lang.Double`
  * `java.lang.Float`
  * `java.lang.Integer`
  * `java.lang.Long`
  * `java.lang.Math`
  * `java.lang.Number`
  * `java.lang.Short`
  * `java.lang.String`
  * `java.lang.StringBuilder`
  * `java.util.ArrayList`
  * `java.util.Arrays`
  * `java.util.Iterator`
  * `java.util.ListIterator`
  * `java.util.Random`
* You are only allowed to access fields (i.e. class variables and instance variables) in the classes you have implemented (in the list of submission files above) and library classes listed below, and constants, in all your submission classes except `CustomCritter`.
  Excluding in `CustomCritter`, accessing fields in other library classes that are not constants will result in a 0.
  * `Critter`
  * `Critter.Attack`
  * `Critter.Direction`
  * `java.awt.Color`
* You are free to use any library methods or fields in `CustomCritter.java`, but with the following exceptions. You are prohibited from calling methods or accessing fields in:
  * Any other classes provided by `CritterGUI.jar` than the allowed ones `Critter`, `Critter.Attack`, `Critter.Direction`, and `CritterInfo`
  * Any classes inside the package `java.lang.reflect` and `java.net`
  * The classes `java.lang.Class`, `java.lang.ClassLoader`, `java.lang.ClassValue`, `java.lang.Compiler`, and `java.security.SecureClassLoader`
  * Other classes with methods that may undermine security (will throw exceptions during tests if used)
