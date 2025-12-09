# GENCG HS25 Journal by Kayleigh Waser

# Week 02
For our first exercise, we had a look at grids. With the inspiration provided in the lecture I started with a basic draft. Since I do not have a coding background, I had some help from ChatGPT and Google Gemini to translate my ideas into code. The first one ended up like this:

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

Put into p5.js, the code turned into that:

{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/_pKU21a6k" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}


# Week 03 
To combine the grid and time exercises of this and last week, I tried programming a rough and simple moon phase cycle in the p5-editor. For now, I restrict myself to simple shapes until I have more practise with the new tools. The code used looked as follows:

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

{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/RjQTIRIfo" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}

This experiment gave me some programming ideas while I worked on more experiments of my designs and patterns. Unfortunately, I have no idea how to upload gifs or images onto this journal yet, so the coding strips and descriptions will have to do.
What I would like to do is to have a dial animation, meaning the moons move in a circle while the lines ease in and out. 
My sketches and ideas will be published, as soon as I find out how.


# Week 04 
This week, we yet again look at time as a topic. The first artworks that come to mind are - of course - by Salvador Dalí with his melting clocks and a video installation I have spotted right next to the Paddington Station in London. It shows a man painting, erasing and repainting the minute hand on a regular train station's clock. A quick online search told me, that said installation is by Maarten Baas and part of the series "Real Time Clock".

https://www.framedcanvasart.com/wp-content/uploads/2024/12/The-Persistence-of-Memory-Melting-Clocks-Painting-Salvador-Dali.jpg 
(Salvador Dalí - The Persistence of Memory (Melting Clocks))

https://www.youtube.com/watch?v=TigeOpy5-TA 
(Marten Baas - Real Time Clock (recording of the installation at the Paddington Station in London,UK))

I, then went online again to research art pieces about time and its passage. During this brief search, I discovered an installation by Maya Lin, titled "Eclipsed Time". Exhibited in New York, a disk installed in the ceiling causes an eclipse in intervals, demonstrating the passage of time.

https://uploads5.wikiart.org/00109/images/maya-lin/eclipsed-time-1989-95.jpg 
(Maya Lin - Eclipsed Time)

For my project, I would like to incorporate lunar cycles as a representation of time, ideally combine it with some sort of pendulum, like an old grandfather clock. Another object to draw inspiration from would be sun dials, from those one may find in a garden to the ancient ones created by indigenous tribes such as the Aztecs.

https://www.reddit.com/media?url=https%3A%2F%2Fpreview.redd.it%2Fd9t0yjd55hg61.jpg%3Fwidth%3D640%26crop%3Dsmart%26auto%3Dwebp%26s%3Df860bf3b48f0d15dc8cce429ae663d03cbc92846
(The Aztec Sun Stone displayed at the National Anthropology Museum in Mexico City)


# Week 05 
In today's lecture, we had a look at automated drawing machines. My immediate train of thought went to paint can artwork, in which a container filled with paint is hung on a rope and released onto a canvas. The released paint creates a seemingly infinte path as as the container swings like a pendulum. 

https://lh3.googleusercontent.com/Mv6LgdJedWVizMWZUveLO1pEjBydQ9Njk5KiQf8FivjlnNoDJgQfJC5fdn5GgjC70Tyjk-LxamS3lMCJZYoMBckQssOtukIQDVs8xpLXzSWwC37grEhPQJAMsY5Dj-H9kIMuvgnCHPc0nXu2yfY8Sg_NVpNnbF3KVxqVH6M6PUjrdaG0TgoOycYAew 
https://i.etsystatic.com/24858016/r/il/86d371/2602609981/il_1588xN.2602609981_r9wp.jpg 
(a few examples I have spotted on google images)

I took this idea into p5.js and programmed a pendulum there, which creates a flower-like pattern in tones of blue and purple.
{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/knUcwVDIa" width="800" height="800" frameborder="no"></iframe>
{% endraw %}

The code I used looks as follows: 
let t = 0;                 // time variable for pendulum motion
let rotation = 0;          // overall rotation angle of the figure-eight
let hueShift = 200;        // color hue (blue start)
let cycleCount = 0;
let lastSign = 1;
let centerX, centerY;

function setup() {
  createCanvas(800, 800);
  colorMode(HSB, 360, 100, 100);
  background(0);
  noFill();
  strokeWeight(2);
  centerX = width / 2;
  centerY = height / 2;
}

function draw() {
  // Parameters
  let swingSpeed = TWO_PI / 2;   // 2 seconds per full figure-8 loop
  let rotateSpeed = radians(0.7); // slow rotation per frame
  let xAmp = 250;
  let yAmp = 150;

  // Calculate figure-eight path
  let x = sin(t) * xAmp;
  let y = sin(t * 2) * yAmp / 2;

  // Apply slow rotation to entire pendulum system
  let rx = x * cos(rotation) - y * sin(rotation);
  let ry = x * sin(rotation) + y * cos(rotation);
  let drawX = centerX + rx;
  let drawY = centerY + ry;

  // Draw trail line (connect previous and current positions)
  stroke(hueShift, 100, 100, 0.8);
  point(drawX, drawY);

  // Color change per full loop
  let sign = sin(t) > 0 ? 1 : -1;
  if (sign !== lastSign) {
    cycleCount++;
    if (cycleCount % 2 === 0) {
      hueShift += 10;
      if (hueShift > 300) hueShift = 200; // cycle blue→purple
    }
    lastSign = sign;
  }

  // Advance motion and rotation
  t += swingSpeed / 60;
  rotation += rotateSpeed;
}

This mandala shape is created one second at a time, with the swings of the pendulum timed to last one second each.
After the difficulties I have faced in Visual Studio Code while trying to add images and files to this journal, I am glad to have found a way to combine some of themes and tasks of the past weeks.

I wished to fill the canvas some more and tried to find a simple way to add more depth to the design, so I added a second pendulum behind the first. The second pendulum is larger and changes color randomly every second, making that change with every swing.
{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/M7HkOkjYh" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}

A next step could be to add controls for users to change the velocity in which the pendulum circles.

# Week 06 
In today's lecture, we formed groups to exchange our ideas and journals. I still could not upload any media properly, so I continue to work with links. It felt inspiring and motivating to see what some of my classmates have been up to during the past weeks. I received tips on the set up for github.
As for my research and state of the project, I received positive feedback. My latest "sketch" has been called "hypnotic" and "relaxing". I am aware that I need to invest some time in my Javascript coding skills to make my ideas a reality. Even though, not everything works the way it should, I feel like I reached a point where I need to put all my energy and effort into the project rather than making github work.

# Week 07 
With today's topic of faces, I commited myself to programming instead of designing something. I simply aimed for replicating this inspirational image:
https://www.dreamstime.com/vector-art-wireframe-depiction-human-face-representing-d-modeling-facial-recognition-technology-digital-identity-image405942177

My first attempt with the help of AI created a grid but not a face. What I liked about it was that the grid moves along with the mouse. That way, should I be able to model a face through code, spectators could get a good look from all angles.

{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/_IRNlUl9n" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}

Next, I tried to have an actual face, but the way it turned out reminded me more of Pinocchio rather than a real human face. I definitely need to make changes to come near the image I looked up.

{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/loZMh5ZER" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}


On the organisational side, I figured out how to add the P5.js files into this journal, which I am glad about and it fuels my motivation. At least now, I am able to demonstrate in a more direct way what I am experiement with and working on.

# Week 08 
This week, I took the time to sit down and figure out some coding. With the help of AI, I managed to create a base to work with and extend the final project. I tried to implement the moon phase/dial idea into working code. The result is far from satisfying:

{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/qBh9-EqPG" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}

It still needs detail work but this is the closest I got to my vision. What I also would like to integrate is the meditative color pendulum somehow. The whole piece, as I currently envision it, would focus on the topic of time.

# Week 09
This week, we had another sheduled exchange session. I took the opportunity to get some clarity on the expected scale of our project. After an interesting and informative conversation, I drew new strength, motivation and inspiration to move onwards.
On the programming side of things, I tried to correct some flaws in my lunar dial sketch. I managed to get some slight improvements, though I still got a long way to go before it is how I envisioned it.

{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/0EtSBjY5_" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}

Maybe, I will abandon the idea to some extent. I would still like to stay within the topic of "time", its representation and use the moon and stars as an inspiriation. But I would like to include some drawing machine in the concept.
An idea, which just struck me: I would still use the rotating outer ring with the cresent moons and stars. But instead of the sigil-esque symbol in the middle, I could have one moon with a black shape shifting from one side to the other to create the phases of the moon.
On the other hand, I still like the pendulum spirograph sketch I made weeks ago. Maybe the pendulum could visualize the passing of days in the lunar cycle.

# Week 10
As today's lesson was about pixel, I wanted to do a side experiment. I realized, that I never tried to have a practice sketch, where the artwork reacts to the user. So, after many failed attempts, I managed to program a simple blue background and a code which generates star patterned pixels whenever users move their cursor over the canvas.

{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/2UTOkT4ba" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}

The first attempt resulted in a somewhat working code, the image, which I tried to reproduce did not quite worj the way I willed it to.

{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/1lJwVf3X9" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}

As for the current state of the project, I gave up on the moonphases inside the inner circle and removed them for now. Now, I want to find out, what I could possibly have on the inside...

{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/NS8PfTp3V" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}

# Week 11
I experimented with the spirograph inside the lunar cycle. The result I liked the most was this sketch:

{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/ec9bRU4xk" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}

I still feel like it misses a certain extra. Maybe, if spectators were able to influence the piece more, it could be more exciting.

# Week 12
Today, I experimented with interactivity between the art work and the user/spectator. I added a speed-up function to the cycle and a color change upon click to the spirograph. 
Everytime, the user moves their cursor to the right side of the canvas, "time will speed up". If the cursor stays in the middle, the rotation continues normally, and if the cursor is on the left side "time stops", meaning the rotation stops.

{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/bBVwk9opw" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}

# Week 13
