<!DOCTYPE html>
<html>
<head>
  <title>Handwriting Recognition</title>
  <script src="https://cdn.jsdelivr.net/npm/tesseract.js@v2.2.0/dist/tesseract.min.js"></script>
  <style>
    #canvas {
      border: 1px solid black;
    }
  </style>
</head>
<body>
  <canvas id="canvas" width="300" height="300"></canvas>
  <br>
  <button id="clearBtn">Clear</button>
  <button id="recognizeBtn">Recognize</button>
  <br><br>
  <p id="textOutput"></p>
</body>
</html>
// Get the canvas and buttons elements
const canvas = document.getElementById("canvas");
const clearBtn = document.getElementById("clearBtn");
const recognizeBtn = document.getElementById("recognizeBtn");
const textOutput = document.getElementById("textOutput");

// Set the canvas context
const ctx = canvas.getContext("2d");

// Clear the canvas
clearBtn.addEventListener("click", function() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
});

// Recognize the handwriting on the canvas
recognizeBtn.addEventListener("click", function() {
  Tesseract.recognize(canvas)
    .then(function(result) {
      textOutput.innerText = result.text;
    });
});

// Draw on the canvas
let isDrawing = false;

canvas.addEventListener("mousedown", function(e) {
  isDrawing = true;
  ctx.beginPath();
  ctx.moveTo(e.clientX - canvas.offsetLeft, e.clientY - canvas.offsetTop);
});

canvas.addEventListener("mouseup", function() {
  isDrawing = false;
});

canvas.addEventListener("mousemove", function(e) {
  if (isDrawing) {
    ctx.lineTo(e.clientX - canvas.offsetLeft, e.clientY - canvas.offsetTop);
    ctx.stroke();
  }
});
