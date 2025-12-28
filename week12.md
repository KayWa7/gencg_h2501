---
layout: default
title: Week 12
---

# Week 12
Today, I experimented with interactivity between the art work and the user/spectator. I added a speed-up function to the cycle and a color change upon click to the spirograph. 

let spiroColorPalette = [
    [0, 255, 255],   // Neon Cyan (Initial)
    [255, 0, 255],   // Neon Magenta
    [255, 255, 0],   // Neon Yellow
    [50, 255, 50],   // Neon Lime Green
    [255, 165, 0]    // Neon Orange
];
let spiroColorIndex = 0;

function mousePressed() {
  // Cycle to the next color in the palette when the user clicks the canvas
  spiroColorIndex = (spiroColorIndex + 1) % spiroColorPalette.length;
}

Everytime, the user moves their cursor to the right side of the canvas, "time will speed up". If the cursor stays in the middle, the rotation continues normally, and if the cursor is on the left side "time stops", meaning the rotation stops.

// --- Dynamic Rotation Speed Control (MODIFIED for 3 zones) ---
let rotationSpeed;
const oneThird = width / 3;

if (mouseX < oneThird) {
  // Left Third: Rotation stops
  rotationSpeed = 0.0;
} else if (mouseX < 2 * oneThird) {
  // Middle Third: Normal rotation (using the original default speed)
  rotationSpeed = 0.2;
} else {
  // Right Third: Fast rotation
  rotationSpeed = 0.4;
}

rotationAngle += rotationSpeed; // Apply the dynamic speed (MODIFIED)

{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/bBVwk9opw" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}