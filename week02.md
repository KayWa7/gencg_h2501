---
layout: default
title: Week 02
---

# Week 02
For our first exercise of the day, we had a look at grids. With the inspiration provided in the lecture I started with a basic draft. Since I do not have a coding background, I had some help from ChatGPT, Google Gemini AI and p5.js References (https://p5js.org/reference/) to translate my ideas into code. This exercise gave me some basic understand of how to set up the canvas, to include shapes, colors and arrange placements and layers. The final sketch ended up with the code like this:

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