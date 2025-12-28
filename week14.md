---
layout: default
title: Week 14
---

# Week 14
The final stage of the project looks as follows:

{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/5qDxTFOEa" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}

The code used is a patchwork of different sketches, overviewed and simplified by Google Gemini AI, looks as follows:

// ==========================================================
// --- GLOBAL PARAMETERS & SETUP (MUST BE DEFINED HERE) ---
// ==========================================================

// --- 1. Background (Segmented, Swinging) Spirograph Parameters (FROM CODE 1) ---
let spiroR = 200; // Fixed circle radius (R)
let spiro_r = 80; // Rolling circle radius (r)
let spiro_d = 110; // Pen distance (d)
let spiroAngle = 0;
const SPIRO_T_INCREMENT = 0.5;
const SWING_FREQUENCY_FACTOR = 360 / 1000; // Swing speed

// --- Path Storage & Sync (Background) ---
let spiroPath = []; // Stores completed, colored segments
let currentPath = []; // Stores the current live segment

// --- 2. Foreground (Continuous, Rotating) Spirograph Parameters (FROM CODE 2) ---
let foreSpiroR = 145; 
let foreSpiro_r = 60; 
let foreSpiro_d = 80; 
let foreSpiroAngle = 0; 
let foreSpiroPath = []; 
const FORE_SPIRO_ANGLE_INCREMENT = 3.0;
const FORE_SPIRO_MAX_LENGTH = 10800; // Path length limit

// --- COLOR & ROTATION CONSTANTS ---
const CANVAS_BACKGROUND_COLOR = [0, 0, 100]; // Dark blue
const ROTATING_BAND_COLOR = [0, 0, 70]; // Darker blue for band

// Background Spirograph Color Palette (Cycle every second)
let spiroColorPalette = [
    [255, 255, 255], [173, 216, 230], [0, 191, 255], [30, 144, 255], [0, 0, 255], [0, 0, 128]
];
let spiroColorIndex = 0;
let lastColorChange = 0;
let lastSegmentColor;
let currentDrawingColor;

// Foreground Spirograph Color Palette (Cycle on mouse click)
let foreSpiroColorPalette = [
    [255, 255, 255], [173, 216, 230], [0, 191, 255], [30, 144, 255], [0, 0, 255], [0, 0, 128]
];
let foreSpiroColorIndex = 0;
let currentForeSpiroColor;

let rotationAngle = 0; // Outer Dial/Foreground Spirograph rotation

// ==========================================================
// --- SETUP FUNCTION ---
// ==========================================================

function setup() {
    createCanvas(500, 500);
    angleMode(DEGREES);
    
    // Background Spiro init
    spiroPath = [];
    currentPath = [];
    lastColorChange = millis();
    currentDrawingColor = spiroColorPalette[spiroColorIndex];
    lastSegmentColor = currentDrawingColor;

    // Foreground Spiro init
    foreSpiroPath = [];
    currentForeSpiroColor = foreSpiroColorPalette[foreSpiroColorIndex];

    frameRate(60);
}

// ==========================================================
// --- HELPER FUNCTIONS ---
// ==========================================================

// Function to draw a 5-pointed star
function drawStar(x, y, radius1, radius2, npoints) {
    let angle = 360 / npoints;
    let halfAngle = angle / 2.0;
    beginShape();
    for (let a = 0; a < 360; a += angle) {
        let sx = x + cos(a) * radius2;
        let sy = y + sin(a) * radius2;
        vertex(sx, sy);
        sx = x + cos(a + halfAngle) * radius1;
        sy = y + sin(a + halfAngle) * radius1;
        vertex(sx, sy);
    }
    endShape(CLOSE);
}

// Handle color change for the FOREGROUND path on mouse click
function mousePressed() {
    foreSpiroColorIndex = (foreSpiroColorIndex + 1) % foreSpiroColorPalette.length;
    currentForeSpiroColor = foreSpiroColorPalette[foreSpiroColorIndex];
}

// ==========================================================
// --- DRAW FUNCTION (WITH CORRECTED LAYERING) ---
// ==========================================================

function draw() {
    
    // 1. Canvas Background (Layer 1)
    background(CANVAS_BACKGROUND_COLOR); 

    // --- BACKGROUND SPIROGRAPH: Color and Path Transfer Logic (Cycle every 1s) ---
    if (millis() > lastColorChange + 1000) {
        if (currentPath.length > 0) {
            spiroPath.push({ points: currentPath, color: lastSegmentColor });
        }
        spiroColorIndex = (spiroColorIndex + 1) % spiroColorPalette.length;
        currentDrawingColor = spiroColorPalette[spiroColorIndex];
        lastSegmentColor = currentDrawingColor;
        currentPath = [];
        lastColorChange = millis();
    }

    // ==========================================================
    // --- 2. BACKGROUND SPIROGRAPH DRAWING (Layer 2) ---
    // ==========================================================
    push();
    translate(width / 2, height / 2);
    noFill();
    strokeWeight(2);
    
    // A. Draw all stored (completed) path segments
    for (let segment of spiroPath) {
        stroke(segment.color); 
        beginShape();
        for (let point of segment.points) {
            vertex(point.x, point.y);
        }
        endShape();
    }

    // B. Calculate and Draw LIVE (current) segment
    let swingAngle = sin(millis() * SWING_FREQUENCY_FACTOR) * 45; 
    let t = currentPath.length * SPIRO_T_INCREMENT; 
    
    // Spirograph (Hypotrochoid) formulas
    let x = (spiroR - spiro_r) * cos(t) + spiro_d * cos(((spiroR - spiro_r) / spiro_r) * t);
    let y = (spiroR - spiro_r) * sin(t) - spiro_d * sin(((spiroR - spiro_r) / spiro_r) * t);

    // Apply the 'swing' rotation (around the center)
    let swungX = x * cos(swingAngle) - y * sin(swingAngle);
    let swungY = y * cos(swingAngle) + x * sin(swingAngle); 

    currentPath.push({ x: swungX, y: swungY });

    // Draw the live segment
    stroke(currentDrawingColor); 
    beginShape();
    for (let i = 0; i < currentPath.length; i++) {
        vertex(currentPath[i].x, currentPath[i].y);
    }
    endShape();
    pop();
    
    // ==========================================================
    // --- 3. FOREGROUND LAYERS: Masking and Dial (Layer 3 & 4) ---
    // ==========================================================
    
    // --- OPAQUE MASK (Covers the outer parts of the background spirograph) ---
    noStroke();
    fill(CANVAS_BACKGROUND_COLOR); 
    ellipse(width / 2, height / 2, 430, 430); 
    
    // --- Outer Rotating Dial (BAND, Stars, and Crescent Moons) ---
    push();
    translate(width / 2, height / 2);
    rotate(rotationAngle); 

    const OUTER_BAND_DIAMETER = 420;
    const INNER_BAND_DIAMETER = 414; 
    let outerRadius = 180;
    let crescentMoonSize = 30;
    let starSize = 15;
    
    // A. DRAW THE WIDE BAND BASE
    noStroke();
    fill(ROTATING_BAND_COLOR); 
    ellipse(0, 0, OUTER_BAND_DIAMETER, OUTER_BAND_DIAMETER); 

    // B. DRAW THE WHITE RIM
    stroke(255); 
    strokeWeight(3);
    noFill();
    ellipse(0, 0, INNER_BAND_DIAMETER, INNER_BAND_DIAMETER); 

    // C. Draw Moons and Stars 
    for (let i = 0; i < 8; i++) {
        let angle = i * 45;
        let x_moon = outerRadius * cos(angle);
        let y_moon = outerRadius * sin(angle);

        // Draw black circle
        fill(0); 
        noStroke();
        ellipse(x_moon, y_moon, crescentMoonSize, crescentMoonSize);
        
        // Use the band's color to cut out the crescent
        fill(ROTATING_BAND_COLOR); 
        ellipse(x_moon - crescentMoonSize / 4, y_moon, crescentMoonSize, crescentMoonSize);
        
        // Draw star
        angle = (i * 45) + 22.5; 
        let x_star = outerRadius * cos(angle);
        let y_star = outerRadius * sin(angle);
        fill(255); 
        noStroke();
        drawStar(x_star, y_star, starSize / 2, starSize / 4, 5);
    }
    pop(); // End of Outer Rotating Dial transformations

    // --- CENTRAL INNER MASK (To mask the center) ---
    noStroke();
    fill(CANVAS_BACKGROUND_COLOR); 
    ellipse(width / 2, height / 2, 290, 290); 

    // --- CENTRAL OUTLINE (The final geometric element before the top layer) ---
    stroke(255); 
    strokeWeight(3);
    noFill();
    ellipse(width / 2, height / 2, 300, 300); 

    // ==========================================================
    // --- 4. FOREGROUND SPIROGRAPH DRAWING (Layer 5 - TOP LAYER) ---
    // ==========================================================
    push();
    translate(width / 2, height / 2);
    rotate(rotationAngle); // Apply the same rotation as the dial!
    
    // A. Calculate the next point (Hypotrochoid)
    let t_fore = foreSpiroAngle; 
    let fore_x = (foreSpiroR - foreSpiro_r) * cos(t_fore) + foreSpiro_d * cos(((foreSpiroR - foreSpiro_r) / foreSpiro_r) * t_fore);
    let fore_y = (foreSpiroR - foreSpiro_r) * sin(t_fore) - foreSpiro_d * sin(((foreSpiroR - foreSpiro_r) / foreSpiro_r) * t_fore); 
    
    // Store the point (relative to the center)
    foreSpiroPath.push({ x: fore_x, y: fore_y });
    
    // B. Draw the continuous path
    noFill();
    stroke(currentForeSpiroColor); 
    strokeWeight(2);
    
    beginShape();
    for (let i = 0; i < foreSpiroPath.length; i++) {
        vertex(foreSpiroPath[i].x, foreSpiroPath[i].y);
    }
    endShape();
    
    // C. Increment the angle & limit path length
    foreSpiroAngle += FORE_SPIRO_ANGLE_INCREMENT; 
    if (foreSpiroPath.length > FORE_SPIRO_MAX_LENGTH) { 
        foreSpiroPath.shift(); // Remove the oldest point
    }

    pop(); // End of Foreground Spirograph transformations
    
    // --- Dynamic Rotation Speed Control (Must be after all rotation-dependent drawing) ---
    let rotationSpeed;
    const oneThird = width / 3;

    if (mouseX < oneThird) {
        rotationSpeed = 0.0;
    } else if (mouseX < 2 * oneThird) {
        rotationSpeed = 0.2;
    } else {
        rotationSpeed = 0.4;
    }

    rotationAngle += rotationSpeed; 
}

All sketches, attempts and experiments can be found here: 
https://editor.p5js.org/KayWa7/sketches.