---
layout: default
title: Week 10
---

# Week 10
As today's lesson was about pixel, I wanted to do a side experiment. I realized, that I never tried to have a practice sketch, where the artwork reacts to the user. So, after many failed attempts, I managed to program a simple blue background and a code which generates star patterned pixels whenever users move their cursor over the canvas.

{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/2UTOkT4ba" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}

// --- Configuration ---
const CANVAS_WIDTH = 800; // Size of the canvas
const CANVAS_HEIGHT = 600; // Size of the canvas
const REVEAL_BLOCK_SIZE = 10; // The size of the "pixel" or block that is revealed
let BG_COLOR; // Will hold the p5.js deep blue color object

// A Set to efficiently store the coordinates of all revealed blocks
let revealedBlocks = new Set(); 

/**
 * The p5.js setup function: runs once when the sketch starts.
 */
function setup() {
    // Creates the main canvas
    createCanvas(CANVAS_WIDTH, CANVAS_HEIGHT);
    
    // Explicitly define the background color: Deep monochrome blue (RGB)
    BG_COLOR = color(10, 20, 50); 
    
    background(BG_COLOR); 
    noStroke(); // Stars will be solid white shapes
    frameRate(60);
}

/**
 * The p5.js draw function: this is kept empty because we only draw when the mouse moves.
 */
function draw() {
    // Drawing logic is in mouseMoved() for instant, cursor-driven response.
}

/**
 * A utility function to draw a 5-pointed star (pentagram).
 * @param {number} x - Center X coordinate.
 * @param {number} y - Center Y coordinate.
 * @param {number} radius - The radius of the star.
 */
function drawPentagram(x, y, radius) {
    const numPoints = 5;
    const angleStep = TWO_PI / numPoints; 
    const halfRadius = radius / 2.5; // Inner radius for the star shape

    beginShape();
    // Loop through 5 points, drawing both the outer and inner vertices
    for (let i = 0; i < TWO_PI; i += angleStep) {
        // Outer point
        vertex(x + cos(i - HALF_PI) * radius, y + sin(i - HALF_PI) * radius);
        // Inner point
        vertex(x + cos(i - HALF_PI + angleStep / 2) * halfRadius, y + sin(i - HALF_PI + angleStep / 2) * halfRadius);
    }
    endShape(CLOSE);
}

/**
 * p5.js event function triggered every time the mouse moves.
 * This function contains the generative drawing logic.
 */
function mouseMoved() {
    // Check if cursor is inside the canvas
    if (mouseX < 0 || mouseX > width || mouseY < 0 || mouseY > height) {
        return;
    }

    // 1. Calculate the top-left corner of the current 10x10 block
    const blockX = floor(mouseX / REVEAL_BLOCK_SIZE) * REVEAL_BLOCK_SIZE;
    const blockY = floor(mouseY / REVEAL_BLOCK_SIZE) * REVEAL_BLOCK_SIZE;
    
    // 2. Create a unique key for this block
    const key = `${blockX},${blockY}`;

    // 3. Optimization: Only draw if this block has NOT been revealed yet
    if (revealedBlocks.has(key)) {
        return; 
    }

    // 4. Reveal a 3x3 area around the cursor (current block + 8 neighbors)
    const radiusBlocks = 1; 
    for (let dx = -radiusBlocks; dx <= radiusBlocks; dx++) {
        for (let dy = -radiusBlocks; dy <= radiusBlocks; dy++) {
            const neighborX = blockX + (dx * REVEAL_BLOCK_SIZE);
            const neighborY = blockY + (dy * REVEAL_BLOCK_SIZE);
            const neighborKey = `${neighborX},${neighborY}`;

            // Check boundaries and check if this specific neighbor block is new
            if (neighborX >= 0 && neighborX < width && 
                neighborY >= 0 && neighborY < height &&
                !revealedBlocks.has(neighborKey)) {
                
                revealedBlocks.add(neighborKey); // Mark as revealed

                // Calculate the center of the block for drawing the star
                const neighborCenterX = neighborX + REVEAL_BLOCK_SIZE / 2;
                const neighborCenterY = neighborY + REVEAL_BLOCK_SIZE / 2;
                
                // Set drawing style: White with random opacity for variation
                fill(255, 255, 255, random(150, 255)); 
                
                // Draw the generative star (pentagram)
                const starRadius = (REVEAL_BLOCK_SIZE / 3) * random(0.8, 1.2); // Random size variation
                drawPentagram(neighborCenterX, neighborCenterY, starRadius);
            }
        }
    }
}

The first attempt resulted in a somewhat working code, the image, which I tried to reproduce did not quite work the way I willed it to.

{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/1lJwVf3X9" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}

// --- p5.js Variables ---
let sourceImage; // The original loaded image
let revealedGraphics; // The p5.Graphics object where the revealed blocks are drawn
let revealedBlocks = new Set(); // Stores the coordinates of revealed blocks as strings ("x,y")
let totalBlocks = 0; // Total number of blocks in the grid
const BLOCK_SIZE = 10; // The size of the pixel blocks (10x10)
let lastBlockX = -1, lastBlockY = -1; // To optimize mouseMoved checks

// --- Image Configuration ---
// IMPORTANT: This path points to the specific London Nightscape image you uploaded.
const imagePath = `uploaded:image_4d7b03.jpg-f3c92249-eceb-4581-ac48-783d6ccae2c3`;
const imageUrl = imagePath;

/**
 * Preloads the image asset before setup is called.
 */
function preload() {
    // Load the image provided by the user
    sourceImage = loadImage(imageUrl, 
        // Success callback
        () => {
            console.log("Image loaded successfully.");
        }, 
        // Failure callback (use a placeholder if loading fails)
        () => {
            console.error("Failed to load image. Using placeholder.");
            // Fallback: create a simple gray image if the upload link is broken
            sourceImage = createGraphics(600, 400);
            sourceImage.background(50, 50, 70);
            sourceImage.textAlign(CENTER, CENTER);
            sourceImage.textSize(32);
            sourceImage.fill(200);
            sourceImage.text("Image Placeholder", 300, 200);
            sourceImage.noStroke();
        }
    );
}

/**
 * Setup function: initializes the canvas and graphics objects.
 */
function setup() {
    // Set canvas size to the image size for a 1:1 match
    const canvasWidth = sourceImage.width;
    const canvasHeight = sourceImage.height;

    // Cap size for better display on small screens
    const maxDimension = Math.min(windowWidth * 0.95, 800);
    let finalWidth = canvasWidth;
    let finalHeight = canvasHeight;

    if (canvasWidth > maxDimension || canvasHeight > maxDimension) {
        const ratio = canvasWidth / canvasHeight;
        if (canvasWidth > canvasHeight) {
            finalWidth = maxDimension;
            finalHeight = maxDimension / ratio;
        } else {
            finalHeight = maxDimension;
            finalWidth = maxDimension * ratio;
        }
    }

    // Create the main canvas
    createCanvas(finalWidth, finalHeight);
    
    // Create a secondary graphics buffer for the revealed image
    // This buffer must have the original image dimensions for correct pixel mapping
    revealedGraphics = createGraphics(sourceImage.width, sourceImage.height);
    revealedGraphics.background(0); // Start completely black
    revealedGraphics.noStroke();
    
    // Calculate total blocks for progress tracking
    totalBlocks = (floor(sourceImage.width / BLOCK_SIZE)) * (floor(sourceImage.height / BLOCK_SIZE));

    // Set the main drawing environment
    pixelDensity(1); // Standardize pixel density for accurate block drawing
    frameRate(60);
}

/**
 * Draw function: handles continuous rendering.
 */
function draw() {
    background(0); // Keep the background black 

    // Draw the revealed graphics buffer scaled to the main canvas size
    image(revealedGraphics, 0, 0, width, height);

    // This section is for the HTML progress bar, which won't run in a pure p5.js editor
    const progressFill = document.getElementById('progress-fill');
    if (progressFill) {
        const progress = (revealedBlocks.size / totalBlocks) * 100;
        progressFill.style.width = `${progress}%`;
    }

    // Stop the loop once the image is fully revealed
    if (revealedBlocks.size >= totalBlocks) {
        noLoop();
    }
}

/**
 * Custom function to handle pixel revelation based on mouse movement.
 */
function mouseMoved() {
    // Only proceed if mouse is over the canvas
    if (mouseX < 0 || mouseX > width || mouseY < 0 || mouseY > height) {
        return;
    }

    // --- 1. Map mouse coordinates to original image coordinates ---
    // Calculate the scaling factor
    const scaleX = sourceImage.width / width;
    const scaleY = sourceImage.height / height;
    
    // Get the mouse coordinates relative to the original image size
    const imgX = floor(mouseX * scaleX);
    const imgY = floor(mouseY * scaleY);

    // --- 2. Determine the current block's top-left coordinate ---
    const blockX = floor(imgX / BLOCK_SIZE) * BLOCK_SIZE;
    const blockY = floor(imgY / BLOCK_SIZE) * BLOCK_SIZE;

    // Optimization: only process if the cursor moved to a new block
    if (blockX === lastBlockX && blockY === lastBlockY) {
        return;
    }
    lastBlockX = blockX;
    lastBlockY = blockY;


    // --- 3. Reveal the current block and neighbors (3x3 area) ---
    const revealRadiusBlocks = 1; // Reveals 3x3 block area around the cursor
    
    for (let dx = -revealRadiusBlocks; dx <= revealRadiusBlocks; dx++) {
        for (let dy = -revealRadiusBlocks; dy <= revealRadiusBlocks; dy++) {
            
            const currentBlockX = blockX + (dx * BLOCK_SIZE);
            const currentBlockY = blockY + (dy * BLOCK_SIZE);

            // Check boundaries against the original image size
            if (currentBlockX >= 0 && currentBlockX < sourceImage.width && 
                currentBlockY >= 0 && currentBlockY < sourceImage.height) {

                const key = `${currentBlockX},${currentBlockY}`;

                if (!revealedBlocks.has(key)) {
                    // This block is new. Add it to the set.
                    revealedBlocks.add(key);

                    // Draw the 10x10 block from the source image onto the revealedGraphics buffer
                    revealedGraphics.copy(
                        sourceImage,  // Source image
                        currentBlockX, currentBlockY, BLOCK_SIZE, BLOCK_SIZE, // Source coords and size
                        currentBlockX, currentBlockY, BLOCK_SIZE, BLOCK_SIZE  // Destination coords and size
                    );
                }
            }
        }
    }
}

/**
 * Handles window resize to keep the sketch responsive.
 */
function windowResized() {
    // Recalculate size based on image and screen dimensions
    const canvasWidth = sourceImage.width;
    const canvasHeight = sourceImage.height;

    const maxDimension = Math.min(windowWidth * 0.95, 800);
    let finalWidth = canvasWidth;
    let finalHeight = canvasHeight;

    if (canvasWidth > maxDimension || canvasHeight > maxDimension) {
        const ratio = canvasWidth / canvasHeight;
        if (canvasWidth > canvasHeight) {
            finalWidth = maxDimension;
            finalHeight = maxDimension / ratio;
        } else {
            finalHeight = maxDimension;
            finalWidth = maxDimension * ratio;
        }
    }
    resizeCanvas(finalWidth, finalHeight);
}

As for the current state of the project, I gave up on the moonphases inside the inner circle and removed them for now. I removed everything in the code which generated the lunar cycle in the middle. Now, I want to find out, what I could possibly have on the inside...

{% raw %}
<iframe src="https://editor.p5js.org/KayWa7/full/NS8PfTp3V" width="100%" height="450" frameborder="no"></iframe>
{% endraw %}