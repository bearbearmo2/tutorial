# PUZZLE PLATFORMER TUTORIAL

## What You Will Make:
By the end of this tutorial you should have a functioning puzzle platformer.
It should allow you to change gravity when you jump and have 4 different blocks that all do different things.

**End Result:** https://puzzle-plat-former--bearbearmo.repl.co/ \
**The Code:** https://repl.it/@bearbearmo/puzzle-plat-former

---

## Disclosure:

In this tutorial you will not learn basic JavaScript, and I will assume you know how to use:

+ variables
+ arrays
+ functions
+ html elements

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

Your script.js file should now look like this:

```javascript
const c = document.getElementById("canvas").getContext("2d");

let currentLevel;

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
0000PP^^bbvv0000
0000000000000000
0000000000000000
0000000000000000
1111111111111111`;

function main() {
  draw();
}

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

function parseLevel(lvl) {
  const toRows = lvl.split("\n");
  const toColumns = toRows.map(r => r.split(""));
  return toColumns;
}

window.onload = function () {
  currentLevel = parseLevel(level);
  main();
}

```
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
  gconst: 9.8
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
You should now have a red square appear wherever you placed your "&", Your a Mamma.

---

##


