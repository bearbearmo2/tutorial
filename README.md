# PUZZLE PLATFORMER TUTORIAL

## What You Will Make:
By the end of this tutorial you should have a functioning puzzle platformer that allows the player to change gravity.
It should allow you to change gravity when you jump and have 4 different blocks that all do different things:

+ Plain Solid Block (does nothing)
+ One Way Gates (only allows you to pass one way - vertical only)
+ Sticky Block (no gravity changing)
+ Anti-grav Block (changes your gravity)

**End Result:** https://Puzzle-Platformer--bearbearmo.repl.co
**The Code:** https://repl.it/@bearbearmo/Puzzle-Platformer

---

## Disclosure:

This is likely not going to be the absolute best way to create a puzzle platformer, however I have done my best to make it simple but effective (which is very difficult).

In this tutorial you will **not** learn basic JavaScript concepts.

However if you wish to do so, I reccomend going to:https://www.w3schools.com/js/
It can show you almost everything, easily and conveniantly.

Also, it will be best if you follow along using a **html + css + js repl** as it automatically sets up everything for you, files, boilerplate stuff and makes it easy for you to code your dreams.

---

## Beginning The Journey

To start of with creating a puzzle platformer we have to do the html; go to your index.html file and between the ``` <body> ``` and ``` <script> ``` tags add a canvas element, like so:

```html
<body>
  
  <canvas id="canvas" width="512" height="512"></canvas>
                                          
  <script src="script.js"></script>
</body>
```
The id can be set to anything you want but for simplicity I will use "canvas". The ```width``` and ```height``` can be set to anything you like aslong as it is a multiple of 32 (512 is 16x32).And that is the only but most important html you will need to do.

---

## The First Steps

The very first thing to do is to go to your **script.js** file on the **Left Hand Side**.

The first few things will be the easiest parts of this tutorial so they will be bundled into 1.

### Defining Your Canvas

 To define our canvas we will use ```const``` instead of ```var``` or ```let``` because we do not want to change the value.\
 To define the canvas we do :
 
 ```javascript
 const c = document.getElementById("canvas").getContext("2d");
 ```
 I have called the constant c so that it is short as I will be using it **a lot**. The ```getContext("2d")``` lets the program know to make the objects 2d (rather than 3d).
 
### Creating Your Level

Creating your level is incredibley easy, all you have to do is create another constant ``` const ``` and call it level. However, this next part is important to follow or it will create real problems later on. You must make a string and fill it with (your canvas height divided by 32) rows of (your canvas width divided by 32) characters, creating (in my case) a 16 by 16 grid - fill it with "0"s but lines of "1" at the top and bottom and finaly add "bb^^PPvv" somewhere into the grid, like so:

```javascript
const level = `1111111111111111
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000bb^^PPvv0000
0000000000000000
0000000000000000
0000000000000000
1111111111111111`;
```
### Splitting Apart Your Nice New Level

To use your new level we need to create our first function, this function needs to:

+ split each row apart
+ split each character of each row apart
+ return the resulting 2d array

Luckily enough, javascript has two built in functions ``` split() ``` and ``` map() ``` we are going to utilise both of these to split our level. Like so:

```javascript
function parseLevel(lvl) {
  const toRows = lvl.split("\n");
  const toColumns = toRows.map(r => r.split(""));
  return toColumns;
}
```
Putting everything so far together we get this:
```javascript
const c = document.getElementById("canvas").getContext("2d");

const level = `1111111111111111
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000bb^^PPvv0000
0000000000000000
0000000000000000
0000000000000000
1111111111111111`;

function parseLevel(lvl) {
  const toRows = lvl.split("\n");
  const toColumns = toRows.map(r => r.split(""));
  return toColumns;
}

// if you want to see the output of the function do: console.log(parselevel(level));
```
---
## Running The Program

Now we have a program it would be usefull if we could run it; to do this we just add 2 new functions :
```javascript
let currentLevel;

// put under the level constant
function main() {
}

//keep at the bottom of the file
window.onload = window.onload = function () {
  currentLevel = parseLevel(level);
  main();
}
```
The first function is empty but will be filled up later on. The second function is activated when the window is loaded, it activates the entirety of the code. I'm also sure you noticed the ``` let currentlevel; ``` at the top of the extract, this new variable is needs to be introduced in the global domain, this is because we need to use it in many different functions. ```let``` allows us to change the value like a normal variable but dissallows the variable to be used before it is defined, stopping bugs that may arrise later on. The semi-colon allows us to define a new variable but without giving it a value until we are ready.

---
## Getting To See Your Hard Work

Now we have our level split up and ready to utilate, we have to utilate it. Obviosly we want to draw it, so it is important to get familiar with ```c.fillStyle``` and ```c.fillRect(x, y, w, h)``` . ```c.fillStyle``` allows you to change the colour of the rectangles you fill. ```c.fillrect(x, y, w, h)``` draws a rectangle onto the canvas in the colour defined in ```c.fillStyle```  the brackets contain the x coordinate, ycoordinate, width, height. Now we know how to draw the level, we can  create a function to do so. We are going to have to draw a block at a time and so we will need a for loop, however because our array is 2d we will need two for loops:

```javascript
function draw() {
  for (let row = 0; row < currentLevel.length; row++) {
    for (let col = 0; col < currentLevel[0].length; col++) {
    }
  }
}
```
This allows us to access each blocks coordinates seperately, and so we can now easily, check the type of block("1","p","b","^","v"), select the colour for that block("black","pink","blue","grey"&"grey")and draw that block with it's coordinates(x(colx32),y(rowx32)) and size(w(32),h(32)). shown here:

```javascript
function draw() {
  for (let row = 0; row < currentLevel.length; row++) {
    for (let col = 0; col < currentLevel[0].length; col++) {
      if (currentLevel[row][col] === "1") {
        c.fillStyle = "black";
        c.fillRect(col * 32, row * 32, 32, 32);
      }

      if (currentLevel[row][col] === "v") {
        c.fillStyle = "grey"
        c.fillRect(col * 32, (row * 32) + 16, 32, 16);
      }

      if (currentLevel[row][col] === "^") {
        c.fillStyle = "grey"
        c.fillRect(col * 32, row * 32, 32, 16);
      }

      if (currentLevel[row][col] === "P") {
        c.fillStyle = "pink"
        c.fillRect(col * 32, row * 32, 32, 32);
      }

      if (currentLevel[row][col] === "b") {
        c.fillStyle = "blue"
        c.fillRect(col * 32, row * 32, 32, 32);
      }
    }
  }

}
```
**note:** It is worth mentoining now that the "^" blocks are one way gates upwards and the "v" blocks are one way gates downwards. This means they have to be half the height of a normal block (16) and the "v" needs to be on the bottom half of the block so has 16(half a block) added to its y coordinate.

To see the output of our draw function we just need to add ```draw();``` to our ```main()``` function, and press run. The output should be a picture of your levels constant.

---

## Acting God

Once we've got our level, we need to make a player. In this case a red square. This squares gonna need some associated values, for this we're going to create an object:

```javascript
const player = {
  x: 0,
  y: 0,
  width: 32,
  height: 32,
  
  //used later on
  gpe: 0,
  yke: 0,
  mass: 64,
  speed: 3,
  gfldstr: 9.8
}
```
Whith the players coordinates and size we can easily draw it in the draw() function:
```javascript
function draw(){
c.fillStyle = "red"
c.fillRect(player.x, player.y, player.width, player.height)
...}
```
This draws in the square when we press run, however you cannot see it because it's coordinates are inside a block. To fix this we just need to add in a few more things. First of all in this example the player will be represented as "&", you need to write your symbol into the level constant(anywhere you like), for my example I'm placing it in the bottom left. Then you just need to write a simple if statement into the draw function, like so:

```javascript

function draw() {
  c.fillStyle = "red";
  c.fillRect(player.x, player.y, player.width, player.height);
  for (let row = 0; row < currentLevel.length; row++) {
    for (let col = 0; col < currentLevel[0].length; col++) {
      ...
      ...
      if (currentLevel[row][col] === "&") {
        player.x = col * 32 // sets x
        player.y = row * 32 // sets y
        currentLevel[row][col] = "0" // clears the initial position
      }
    }
  }

}
```
You should now have a red square appear wherever you placed your "&". The easy part of this tutorial is now complete. This is also a good time to talk about how we are going to make the player move. Well were going to be changing the coordinates of the player and then drawing it again in it's new coordinates, however to do this were going to have to use something that runs the entire code on a loop. The easiest way to do that is run the function main() on a loop by using ```requestAnimationFrame(main);``` basicly this will run main() every frame that the game is running. However, this creates a problem whenever the player is redrawn the old image is still there.  To fix this, at the start of the draw() function we just need to put ```c.clearRect(0, 0, canvas.width, canvas.height);``` this wipes everything off the screen before we draw it back just after, this is a little inefficient as the whole level is redrawn every frame. To make this better we **could** just clear the players previous coordinates, but this creates a whole new set of problems that I'm not going to deal with in this tutorial. Just take my word for it, this way is better.

---

## Gravity

This is going to be the hardest part of the tutorial but keep whith it and you'll've made your own game.
To create gravity we have to create a new function ```calcGPE(obj)``` (it isn't important that you know the physics involved, however a deeper understanding of the physics used here could be helpfull for knowing what I'm doing.) In this function we need to find the vertical kinetic energy (YKE) , to do this we need to find the gravitational potential energy (GPE); the formula for GPE is GPE = GMH (Gravitational Feild Strength (obj.gfldstr/1000000) x Mass (obj.mass) x Height ((512 - obj.height) - (obj.y / 32)). This formula allows us to find the GPE of any object. This means our final function is:

```javascript
function calcGPE(obj) {
  return obj.mass * (obj.gfldstr / 1000000) * ((512 - obj.height) - (obj.y / 32));
}
```
The feild strength is divided by 1000000 because normal the height is in metres but in this measurement is in pixels so we need to account for that by dividing the feild strength by 1 million.

To make gravity act upon our player we need to create **another function**, ``` gravity(obj)``` inside this new function we need to do 3 things take the kinetic energy from the y coordinate, take the gpe from the kinetic enrgy, find the gpe:

```javascript
function gravity(obj) {
  obj.y -= obj.yke;
  obj.yke -= obj.gpe;
  obj.gpe = calcGPE(obj);
  }
```

Now, if we add ```gravity(player);``` to the main function our player should fall out of the sky.\

---

## Vertical Collisions

We want our player to **collide** with the blocks and **stop**, to do this we need to check the block bellow our player, check what kind of block it is and then stop the block if neccesary. However, we don't yet have an easy way of checking what a block is, so we create a new function, getTile(x, y):

```javascript
function getTile(x, y) {
//checks the block your looking for is inside the level
  if (x < currentLevel.length * 64 && x > 0 && y < currentLevel[0].length * 32 && y > 0) {
    return currentLevel[Math.floor(y / 32)][Math.floor(x / 32)];//returns the block
  }
}
```
Now we have that, we can easily check the block at whatever coordinates we want.\

To check the block under us is one we want solid, we just need a simple if statement:

```javascript
if (getTile(obj.x + 32, (obj.y + 32)) === "1" || getTile(obj.x, (obj.y + 32)) === "1") {

}
```
To stop the player we need to set it's yke to 0 and round its y coordinate to the nearest 32:

```javascript
if (getTile(obj.x + 32, (obj.y + 32)) === "1" || getTile(obj.x, (obj.y + 32)) === "1") {
   if (obj.yke <= 0) {
      obj.yke = 0;
      obj.y -= (obj.y % 32);
   }
}
```
The if statement just allows the player to leave the block again when we move up.

Now put this into the gravity function, make sure it's under everything allready in there.\
We can use this function again for the other blocks (apart from "v" because it allows us to move through it when coming from above.):

```javascript
function gravity(obj) {
...
...
// has the "^" included because we want it to do exactly the same thing for that block
if (getTile(obj.x + 32, (obj.y + 32)) === "1" || getTile(obj.x, (obj.y + 32)) === "1" || getTile(obj.x + 32, (obj.y + 32)) === "^" || getTile(obj.x, (obj.y + 32)) === "^") {
    if (obj.yke <= 0) {
      obj.yke = 0;
      obj.y -= (obj.y % 32);
    }
  }
  else if (getTile(obj.x + 32, obj.y + 32) === "P" || getTile(obj.x, obj.y + 32) === "P") {
    obj.yke = 0;
    obj.y -= (obj.y % 32);
    // no if statement because we don't want the player to leave this block vertically (change gravity)
  }
  else if (getTile(obj.x + 32, obj.y + 32) === "b" || getTile(obj.x, obj.y + 32) === "b") {
    if (obj.yke <= 0) {
      obj.yke = 0;
      obj.y -= (obj.y % 32);
    }
    obj.gfldstr *= -1 // changes gravity
  }
}
```
But of course being able to change gravity means that we need the blocks to be solid when we move upwards as well
This is easy to implement we just need to change the y coordinates that we check from y + 32 to just y. We also need to slow down the player before it meets the block to prevent it clipping in, as the previous rounnding of the coordinates does not work in this situation.
This means we need to add another 3 if statements:

```javascript
function gravity(obj) {
...
...
...
...
else if (getTile(obj.x, obj.y) === "1" || getTile(obj.x + 32, obj.y) === "1" || getTile(obj.x, obj.y) === "v" || getTile(obj.x + 32, obj.y) === "v") {
    if (obj.yke > 0) {
      obj.y += obj.yke;// slows down the player
      obj.yke = 0;
    }
  } 
  else if (getTile(obj.x, obj.y) === "P" || getTile(obj.x + 32, obj.y) === "P") {
    obj.yke = 0; // only has this bit because we don't mind if the player sinks into the block because it is meant to be goo anyway
  }
  else if (getTile(obj.x, obj.y) === "b" || getTile(obj.x + 32, obj.y) === "b") {
    if (obj.yke > 0) {
      obj.y += obj.yke;
      obj.yke = 0;
    }
    obj.gfldstr *= -1
  }
}
```
And thats our vertical collisions finnished, the hardest part is done!

---

## Getting Some exercise

Now we ahve a solid world, it's about time we moved through it. To do this we need to addEventListeners:

```javascript
addEventListener("keydown", function (event) {
  keysDown[event.keyCode] = true;
});

addEventListener("keyup", function (event) {
  delete keysDown[event.keyCode];
});
```
If you're not sure about how to use event listeners go Here:https://www.w3schools.com/js/js_htmldom_eventlistener.asp
Otherwise, you may have noticed keysdone has not been defined, so at the very top of the file under ```let currentlevel;``` put a new object ```let keysDown = {};``` . The first event listener detects any keys currently pressed down and adds the keycode of that key. the second event listener detects any keys that have stopped being pressed down and removes the keycode of that key. This allows us to create a function that checks if the keys we want to be pressed down (w, a, s, d, spacebar) are pressed down, like so:

```javascript
function input() {

  if (65 in keysDown) {
  // checks that there is no block in the way
    if (getTile((player.x - player.speed), player.y + 16) === "0") {
      player.x -= player.speed;// moves to the left
    }
  }

  if (68 in keysDown) {
  //checks that there is no block in the way
    if (getTile(((player.x + player.width) + player.speed), player.y + 16) === "0") {
      player.x += player.speed;// moves to the right
    }
  }
  
  //checks if the player is on the floor
  if (87 in keysDown && player.yke === 0) {
    player.gfldstr = -9.8;// changes gravity
  }
  
  //checks if the player is on the floor
  if (83 in keysDown && player.yke === 0) {
    player.gfldstr = 9.8;// changes gravity
  }

  // restarts the level
  if (32 in keysDown && player.yke === 0) {
    player.gfldstr = 9.8;
    currentLevel = parseLevel(level);
  }
}
```
Now, The very last thing we need to do is add input(); to the main() function.\
Run the repl, and enjoy your new puzzle platformer. Remember you can create **whatever** levels you like and create as **many** levels as you like

# Thank You Very Much

I wish to thank you very much for making it to the end of my tutorial, I hope you enjoyed it.
I also want to inform you that this is based off of **Lucadukeys Basic Platformer Tutorial**, but I did ask Lucadukey personally if I could use his bassis to create my own tutorial and we have agreed that any profit I might get from this (but probably not), he will get **50%**. I would also like to say that I did code this **myself** and that I started develpoment before Lucadukey posted his tutorial. If you have any doubts I'm sure he would be happy to reply to your comments. **Please Upvote** it would mean a lot, and once again, Thankyou very Much.

created by: Bearbearmo.
