## Computing without computer

### Sollewit: Wall drawing

Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

![Example Image](content/day01/test.jpg)

Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua.

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

### Webcam tests

Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua.

{% raw %}
<iframe src="content/day01/01/embed.html" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}

Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua.

{% raw %}
<iframe src="content/day01/02/embed.html" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}

## Computing with computer

Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua.

> At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua.

{% raw %}
<iframe src="content/day01/03/embed.html" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}

* Lorem ipsum dolor sit amet
* Consetetur sadipscing elitr, sed diam nonumy.
* At vero eos et accusam et justo duo dolores et ea rebum. 

# Week 2 Journal by Kayleigh Waser
GENCG Week 2 – Code Grid Exercise
// Variables for animation
let angle = 0;
let pulseSize = 1;
let pulseDirection = 0.01;

function setup() {
  createCanvas(400, 400);
  // Set rectangle mode to center for easier positioning
  rectMode(CENTER);
}

function draw() {
  background(0);
  
  // Create a grid using loops (more efficient than drawing each line individually)
  drawGrid();
  
  // Animate the shapes
  animateShapes();
}

function drawGrid() {
  // Set grid properties
  stroke('purple');
  strokeWeight(1);
  
  // Draw horizontal lines
  for (let y = 0; y <= height; y += 50) {
    line(0, y, width, y);
  }
  
  // Draw vertical lines
  for (let x = 0; x <= width; x += 50) {
    line(x, 0, x, height);
  }
}

function animateShapes() {
  // Update animation variables
  angle += 0.02;
  pulseSize += pulseDirection;
  
  // Reverse pulse direction when it gets too big or too small
  if (pulseSize > 1.5 || pulseSize < 0.5) {
    pulseDirection *= -1;
  }
  
  // Animated blue circle - rotates and pulses
  push(); // Save current drawing state
  translate(100, 100); // Move origin to circle center
  rotate(angle); // Rotate around the center
  fill('blue');
  circle(0, 0, 100 * pulseSize); // Draw circle at new origin
  pop(); // Restore original drawing state
  
  // Animated red ellipse - moves in a circular path
  let ellipseX = 300 + cos(angle * 1.5) * 50;
  let ellipseY = 300 + sin(angle * 1.5) * 50;
  fill('red');
  ellipse(ellipseX, ellipseY, 100, 50);
  
  // Animated triangle - bounces vertically
  let triangleY = 75 + sin(angle * 2) * 25;
  fill('white');
  triangle(300, triangleY, 258, triangleY - 55, 286, triangleY + 25);
  
  // Animated square - changes color and size
  let squareSize = 50 + sin(angle) * 20;
  let squareColor = color(
    150 + sin(angle) * 105,
    100 + cos(angle) * 100,
    200 + sin(angle * 0.7) * 55
  );
  fill(squareColor);
  rect(200, 200, squareSize, squareSize);
}

{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/_pKU21a6k" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}


# Week 3 Journal by Kayleigh Waser
GENCG Week 3 Exercise Grid & Time
To combine the exercises of this and last week, I tried programming a rough and simple moon phase cycle in the p5-editor. For now, I restrict myself to simple shapes until I have more practise with the new tools. The code used looked as follows:

let angle = 0;     // for moon orbit
let lineLength = 50;  // base line length
let t = 0;         // time variable for easing

function setup() {
  createCanvas(500, 500);
  angleMode(DEGREES);
}

function draw() {
  background(87);

  translate(width/2, height/2);

  // Ease the line length (sin wave)
  let easedLength = lineLength + sin(t) * 30;
  t += 2; // speed of easing

  stroke(0);
  strokeWeight(4);

  // Draw radiating lines
  for (let i = 0; i < 6; i++) {
    let x = cos(i * 60) * easedLength;
    let y = sin(i * 60) * easedLength;
    line(0, 0, x, y);
  }

  // Orbiting moons
  let orbitRadius = 150;
  let moonSize = 60;

  for (let i = 0; i < 4; i++) {
    let x = cos(angle + i * 90) * orbitRadius;
    let y = sin(angle + i * 90) * orbitRadius;

    // draw moon base
    noStroke();
    fill(0);
    ellipse(x, y, moonSize);

    // add moon phase mask
    fill(255);
    if (i === 0) {
      // full moon
      ellipse(x, y, moonSize);
    } else if (i === 1) {
      // waning (left shadow)
      ellipse(x + 15, y, moonSize);
    } else if (i === 2) {
      // new moon (fully black, do nothing extra)
    } else if (i === 3) {
      // waxing (right shadow)
      ellipse(x - 15, y, moonSize);
    }
  }

  // make moons orbit
  angle += 1;
}


This experiment gave me some programming ideas while I worked on more experiments of my designs and patterns. Unfortunately, I have no idea how to upload gifs or images onto this journal yet, so the coding strips and descriptions will have to do.
What I would like to do is to have a dial animation, meaning the moons move in a circle while the lines ease in and out. 
My sketches and ideas will be published, as soon as I find out how.


# Week 4 Journal by Kayleigh Waser

This week, we yet again look at time as a topic. The first artworks that come to mind are - of course - by Salvador Dalí with his melting clocks and a video installation I have spotted right next to the Paddington Station in London. It shows a man painting, erasing and repainting the minute hand on a regular train station's clock. A quick online search told me, that said installation is by Maarten Baas and part of the series "Real Time Clock".

I, then went online again to research art pieces about time and its passage. During this brief search, I discovered an installation by Maya Lin, titled "Eclipsed Time". Exhibited in New York, a disk installed in the ceiling causes an eclipse in intervals, demonstrating the passage of time.

For my project, I would like to incorporate lunar cycles as a representation of time, ideally combine it with some sort of pendulum, like an old grandfather clock. Another object to draw inspiration from would be sun dials, from those one may find in a garden to the ancient ones created by indigenous tribes such as the Aztecs.
