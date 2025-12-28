---
layout: default
title: Week 03
---

# Week 03 
To combine the grid and time exercises of this and last week, I tried programming a rough and simple moon phase cycle in the p5-editor. For now, I restrict myself to simple shapes until I have more practise with the new tools. I made a rough sketch in PowerPoint to plan the composition.

<div align="center">
  <img src="images/Week_3_Design_Idea.png" alt="Week 3 Sketch Idea" width="600">
  <p><i>My initial sketch idea: The Lunar Cycle.</i></p>
</div>

The code used to create a basis in p5.js looked as follows:

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

This experiment gave me some programming ideas while I try to find my project by experimenting more with designs and patterns.
What I would like to do is to have a dial animation, meaning the moons move in a circle while the lines ease in and out. Those elements are included in the sketch, yet it lacks colors and is not nice to look at with the odd full moons which out to be crescent shapes.