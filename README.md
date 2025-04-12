<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title id="page-title">Land Area Calculator</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins&display=swap" rel="stylesheet">
<style>
body {
font-family: 'Poppins', sans-serif;
margin: 0;
padding: 10px;
background-color: #f0f0f0;
display: flex;
justify-content: center;
}
.container {
max-width: 600px;
width: 100%;
background: white;
padding: 20px;
border-radius: 10px;
box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}
.watermark {
text-align: center;
font-size: 16px;
color: #999;
margin-bottom: 10px;
}
.toggle-row {
display: flex;
justify-content: space-between;
margin-bottom: 10px;
flex-wrap: wrap;
gap: 4px;
}
.toggle-row button, .toggle-row select {
flex: 1;
margin: 2px;
padding: 10px;
font-size: 14px;
border: none;
background-color: #ddd;
color: black;
border-radius: 5px;
cursor: pointer;
min-width: 100px;
}
.toggle-row button.active {
background-color: #4CAF50;
color: black;
}
#shapeDropdown {
background-color: #ddd;
color: black;
}
#shapeDropdown.active {
background-color: #4CAF50;
color: black;
}
#shapeDropdown option {
background-color: white;
color: black;
}
.language-select {
display: block;
margin: 0 auto 10px auto;
font-size: 14px;
padding: 6px 10px;
border-radius: 5px;
}
h1 {
font-size: 24px;
text-align: center;
margin: 10px 0;
}
label {
font-size: 16px;
display: block;
margin: 10px 0 5px;
}
input {
width: 100%;
padding: 10px;
font-size: 16px;
border: 1px solid #ccc;
border-radius: 5px;
box-sizing: border-box;
}
button {
width: 100%;
padding: 12px;
font-size: 16px;
background-color: #4CAF50;
color: white;
border: none;
border-radius: 5px;
margin: 10px 0;
cursor: pointer;
}
button:hover {
background-color: #45a049;
}
button:disabled {
background-color: #aaa;
cursor: not-allowed;
}
#quadCalcButtons {
display: none;
margin-bottom: 10px;
}
#quadCalcButtons.visible {
display: block;
}
#calcDiagBtn, #calcPatwariBtn {
background-color: #ddd;
margin-bottom: 5px;
color: black;
}
#calcDiagBtn:hover, #calcPatwariBtn:hover {
background-color: #45a049;
}
#calcDiagBtn.active, #calcPatwariBtn.active {
background-color: #4CAF50;
color: black;
}
#result {
font-size: 14px;
line-height: 1.6;
white-space: pre-wrap;
background: #f9f9f9;
padding: 10px;
border-radius: 5px;
border: 1px solid #ddd;
display: none;
margin-top: 10px;
}
#result.error {
color: #dc3545;
background: #f8d7da;
border-color: #f5c6cb;
padding: 15px;
}
.section {
display: none;
}
.visible {
display: block;
}
#printBtn {
display: none;
background-color: #008CBA;
}
#printBtn:hover {
background-color: #007399;
}
#historyBtn {
background-color: #6c757d;
}
#historyBtn:hover {
background-color: #5a6268;
}
#resetBtn {
background-color: #dc3545;
}
#resetBtn:hover {
background-color: #c82333;
}
#clearHistoryBtn {
background-color: #dc3545;
margin-top: 10px;
}
#clearHistoryBtn:hover {
background-color: #c82333;
}
.pond-point, .poly-side, .poly-diag {
display: flex;
gap: 10px;
margin-bottom: 5px;
align-items: center;
}
.pond-point label, .poly-side label, .poly-diag label {
min-width: 70px;
text-align: right;
}
.pond-point input, .poly-side input, .poly-diag input {
flex: 1;
}
#addPondBtn, #addPolySideBtn {
background-color: #28a745;
}
#addPondBtn:hover, #addPolySideBtn:hover {
background-color: #218838;
}
#removePondBtn, #removePolySideBtn {
background-color: #dc3545;
}
#removePondBtn:hover, #removePolySideBtn:hover {
background-color: #c82333;
}
#diagWrapper.hidden {
display: none;
}
#resultCanvas {
display: none;
margin: 10px auto;
}
#historyList {
display: none;
background: #f9f9f9;
padding: 10px;
border-radius: 5px;
border: 1px solid #ddd;
margin-top: 10px;
}
#historyList button {
background-color: #ddd;
color: black;
margin: 5px 0;
}
#historyList button:hover {
background-color: #ccc;
}
button:focus, select:focus, input:focus {
outline: 2px solid #4CAF50;
}
@media screen and (max-width: 300px) {
label { font-size: 14px; }
input { font-size: 14px; padding: 8px; }
button { font-size: 14px; padding: 10px; }
.pond-point label, .poly-side label, .poly-diag label { min-width: 50px; }
}
@media print {
.container > *:not(.watermark, .section.visible, #result) { display: none; }
.section.visible { margin-bottom: 20px; }
#result { border: none; background: none; }
#result.error { background: none; color: #000; border: none; }
#resultCanvas { display: none; }
.watermark { font-size: 12px; }
}
</style>
</head>
<body>
<div class="container">
<div class="watermark" id="watermark">Developed by Sharma Ji Charkhi Dadri</div>

<select id="language" class="language-select" onchange="toggleLanguage()">
<option value="en">English</option>
<option value="hi">à¤¹à¤¿à¤¨à¥à¤¦à¥€</option>
</select>

<h1 id="title">Land Area Calculator</h1>
<p style="text-align:center;" id="info">1 kanal = 20 marla, 1 marla = 9 sarsai</p>

<div class="toggle-row">
<button onclick="selectShape('rect')" id="btn-rect" aria-label="Select Rectangular Shape">Rectangular</button>
<button onclick="selectShape('quad')" id="btn-quad" aria-label="Select Irregular Quadrilateral Shape">Irregular Quadrilateral</button>
<select onchange="handleDropdownShape(this)" id="shapeDropdown" aria-label="Select Additional Shapes">
<option value="" disabled selected id="dropdown-placeholder">More Shapes</option>
<option value="tri">Triangle</option>
<option value="penta">Pentagon</option>
<option value="pond">Pond (Simpsonâ€™s)</option>
<option value="poly">Polygon</option>
</select>
</div>

<div id="quadCalcButtons" class="section">
<button id="calcDiagBtn" onclick="selectQuadMethod('diagonal')" aria-label="Calculate with Diagonal">With Diagonal</button>
<button id="calcPatwariBtn" onclick="selectQuadMethod('patwari')" aria-label="Calculate with Patwari Assumption">Patwari Assumption</button>
</div>

<!-- Rectangle -->
<div id="rectSection" class="section visible">
<label for="rectLength" id="rectLengthLabel">Length (in karam)</label>
<input id="rectLength" type="number" min="1" />
<label for="rectWidth" id="rectWidthLabel">Width (in karam)</label>
<input id="rectWidth" type="number" min="1" />
<label for="rectShare" id="rectShareLabel">Share (e.g. 1 or 3/4)</label>
<input id="rectShare" type="text" />
</div>

<!-- Irregular Quadrilateral -->
<div id="quadSection" class="section">
<label for="north" id="quadNorthLabel">North Side (in karam)</label>
<input id="north" type="number" min="1" />
<label for="east" id="quadEastLabel">East Side (in karam)</label>
<input id="east" type="number" min="1" />
<label for="south" id="quadSouthLabel">South Side (in karam)</label>
<input id="south" type="number" min="1" />
<label for="west" id="quadWestLabel">West Side (in karam)</label>
<input id="west" type="number" min="1" />
<div id="diagWrapper">
<label for="diag" id="quadDiagLabel">Diagonal (in karam)</label>
<input id="diag" type="number" min="1" />
</div>
<label for="irregShare" id="irregShareLabel">Share (e.g. 1 or 2/3)</label>
<input id="irregShare" type="text" />
</div>

<!-- Triangle -->
<div id="triSection" class="section">
<label for="t1" id="triT1Label">Side 1 (in karam)</label>
<input id="t1" type="number" min="1" />
<label for="t2" id="triT2Label">Side 2 (in karam)</label>
<input id="t2" type="number" min="1" />
<label for="t3" id="triT3Label">Side 3 (in karam)</label>
<input id="t3" type="number" min="1" />
<label for="triShare" id="triShareLabel">Share (e.g. 1 or 3/4)</label>
<input id="triShare" type="text" />
</div>

<!-- Pentagon -->
<div id="pentaSection" class="section">
<label for="ab" id="pentaABLabel">Side AB (in karam)</label>
<input id="ab" type="number" min="1" />
<label for="bc" id="pentaBCLabel">Side BC (in karam)</label>
<input id="bc" type="number" min="1" />
<label for="cd" id="pentaCDLabel">Side CD (in karam)</label>
<input id="cd" type="number" min="1" />
<label for="de" id="pentaDELabel">Side DE (in karam)</label>
<input id="de" type="number" min="1" />
<label for="ea" id="pentaEALabel">Side EA (in karam)</label>
<input id="ea" type="number" min="1" />
<label for="d1" id="pentaD1Label">Diagonal 1 (e.g., AC) (in karam)</label>
<input id="d1" type="number" min="1" />
<label for="d2" id="pentaD2Label">Diagonal 2 (e.g., AD) (in karam)</label>
<input id="d2" type="number" min="1" />
<label for="pentShare" id="pentShareLabel">Share (e.g. 1 or 1/2)</label>
<input id="pentShare" type="text" />
</div>

<!-- Pond -->
<div id="pondSection" class="section">
<div id="pondPointsContainer"></div>
<button type="button" onclick="addPondPoint()" id="addPondBtn" aria-label="Add Pond Point">Add Point</button>
<button type="button" onclick="removePondPoint()" id="removePondBtn" aria-label="Remove Pond Point">Remove Point</button>
<label for="distance" id="pondDistanceLabel">Common Distance d (in karam)</label>
<input id="distance" type="number" min="1" />
<label for="pondShare" id="pondShareLabel">Share (e.g. 1 or 3/4)</label>
<input id="pondShare" type="text" />
</div>

<!-- Polygon -->
<div id="polySection" class="section">
<div id="polySidesContainer"></div>
<button type="button" onclick="addPolySide()" id="addPolySideBtn" aria-label="Add Polygon Side">Add Side</button>
<button type="button" onclick="removePolySide()" id="removePolySideBtn" aria-label="Remove Polygon Side">Remove Side</button>
<div id="polyDiagsContainer"></div>
<label for="polyShare" id="polyShareLabel">Share (e.g. 1 or 3/4)</label>
<input id="polyShare" type="text" />
</div>

<button id="calcBtn" onclick="calculate()" aria-label="Calculate Area">Calculate Area</button>
<button onclick="resetForm()" id="resetBtn" aria-label="Reset Form">Reset</button>
<button onclick="toggleHistory()" id="historyBtn" aria-label="View Calculation History" disabled>History</button>
<button id="printBtn" onclick="window.print()" aria-label="Print Results" style="display: none;">Print</button>

<div id="result" aria-live="polite" role="alert"></div>
<div id="historyList">
<button onclick="clearHistory()" id="clearHistoryBtn" aria-label="Clear Calculation History">Clear History</button>
</div>
<canvas id="resultCanvas" style="display: none; margin: 10px auto;"></canvas>
</div>

<script>
/* --- DOM Cache --- */
const domCache = {
  shapeDropdown: document.getElementById('shapeDropdown'),
  result: document.getElementById('result'),
  resultCanvas: document.getElementById('resultCanvas'),
  rectSection: document.getElementById('rectSection'),
  quadSection: document.getElementById('quadSection'),
  triSection: document.getElementById('triSection'),
  pentaSection: document.getElementById('pentaSection'),
  pondSection: document.getElementById('pondSection'),
  polySection: document.getElementById('polySection'),
  printBtn: document.getElementById('printBtn'),
  historyBtn: document.getElementById('historyBtn'),
  historyList: document.getElementById('historyList'),
  quadCalcButtons: document.getElementById('quadCalcButtons')
};

/* --- Translations --- */
const translations = {
  en: {
    title: "Land Area Calculator",
    watermark: "Developed by Sharma Ji Charkhi Dadri",
    info: "1 kanal = 20 marla, 1 marla = 9 sarsai",
    rect: "Rectangular",
    quad: "Irregular Quadrilateral",
    tri: "Triangle",
    penta: "Pentagon",
    pond: "Pond (Simpsonâ€™s)",
    poly: "Polygon",
    calc: "Calculate Area",
    calcDiagonal: "With Diagonal",
    calcPatwari: "Patwari Assumption",
    reset: "Reset",
    print: "Print",
    history: "History",
    clearHistory: "Clear History",
    calculationN: "Calculation {n} ({time})",
    addPoint: "Add Point",
    removePoint: "Remove Point",
    addSide: "Add Side",
    removeSide: "Remove Side",
    dropdownPlaceholder: "More Shapes",
    rectLength: "Length (in karam)",
    rectWidth: "Width (in karam)",
    rectShare: "Share (e.g. 1 or 3/4)",
    quadNorth: "North Side (in karam)",
    quadEast: "East Side (in karam)",
    quadSouth: "South Side (in karam)",
    quadWest: "West Side (in karam)",
    quadDiag: "Diagonal (in karam)",
    irregShare: "Share (e.g. 1 or 2/3)",
    triT1: "Side 1 (in karam)",
    triT2: "Side 2 (in karam)",
    triT3: "Side 3 (in karam)",
    triShare: "Share (e.g. 1 or 3/4)",
    pentaAB: "Side AB (in karam)",
    pentaBC: "Side BC (in karam)",
    pentaCD: "Side CD (in karam)",
    pentaDE: "Side DE (in karam)",
    pentaEA: "Side EA (in karam)",
    pentaD1: "Diagonal 1 (e.g., AC) (in karam)",
    pentaD2: "Diagonal 2 (e.g., AD) (in karam)",
    pentShare: "Share (e.g. 1 or 1/2)",
    pondDistance: "Common Distance d (in karam)",
    pondShare: "Share (e.g. 1 or 3/4)",
    polySide: "Side {side} (in karam)",
    polyDiag: "Diagonal A{point} (in karam)",
    polyShare: "Share (e.g. 1 or 3/4)",
    patwariTrapeziumNote: "Patwari assumption: Trapezium used",
    errorPatwariLargeDiff: "Opposite sides differ by 5 or more karams. Please use the 'With Diagonal' button for accurate calculation.",
    totalArea: "Total Area",
    shareArea: "Share-wise Area",
    errorLength: "Length must be a positive integer.",
    errorWidth: "Width must be a positive integer.",
    errorShare: "Invalid share format.",
    errorSideNorth: "North Side must be a positive integer.",
    errorSideEast: "East Side must be a positive integer.",
    errorSideSouth: "South Side must be a positive integer.",
    errorSideWest: "West Side must be a positive integer.",
    errorDiagonal: "Diagonal must be a positive integer.",
    errorSide1: "Side 1 must be a positive integer.",
    errorSide2: "Side 2 must be a positive integer.",
    errorSide3: "Side 3 must be a positive integer.",
    errorSideAB: "Side AB must be a positive integer.",
    errorSideBC: "Side BC must be a positive integer.",
    errorSideCD: "Side CD must be a positive integer.",
    errorSideDE: "Side DE must be a positive integer.",
    errorSideEA: "Side EA must be a positive integer.",
    errorDiagonal1: "Diagonal 1 must be a positive integer.",
    errorDiagonal2: "Diagonal 2 must be a positive integer.",
    errorDistance: "Common distance must be a positive integer.",
    errorPondPoints: "Pond calculation requires at least 6 points and an even number of points.",
    errorPondHeights: "All pond heights must be positive integers.",
    errorPolySides: "Polygon requires at least 6 sides with positive integers.",
    errorPolyDiags: "Polygon requires at least {n} diagonals with positive integers.",
    errorInvalidTriangle: "Invalid triangle formed by sides.",
    errorInvalidTriangles: "Invalid triangles formed by sides and diagonals.",
    errorInvalidTrapezium: "Invalid trapezium formed.",
    errorGeneral: "Please provide valid inputs for the selected shape."
  },
  hi: {
    title: "à¤­à¥‚à¤®à¤¿ à¤•à¥à¤·à¥‡à¤¤à¥à¤°à¤«à¤² à¤•à¥ˆà¤²à¤•à¥à¤¯à¥‚à¤²à¥‡à¤Ÿà¤°",
    watermark: "à¤¶à¤°à¥à¤®à¤¾ à¤œà¥€ à¤šà¤°à¤–à¥€ à¤¦à¤¾à¤¦à¤°à¥€ à¤¦à¥à¤µà¤¾à¤°à¤¾ à¤µà¤¿à¤•à¤¸à¤¿à¤¤",
    info: "1 à¤•à¤¨à¤¾à¤² = 20 à¤®à¤°à¤²à¤¾, 1 à¤®à¤°à¤²à¤¾ = 9 à¤¸à¤°à¤¸à¤¾à¤ˆ",
    rect: "à¤†à¤¯à¤¤à¤¾à¤•à¤¾à¤°",
    quad: "à¤…à¤¨à¤¿à¤¯à¤®à¤¿à¤¤ à¤šà¤¤à¥à¤°à¥à¤­à¥à¤œ",
    tri: "à¤¤à¥à¤°à¤¿à¤•à¥‹à¤£",
    penta: "à¤ªà¤‚à¤šà¤­à¥à¤œ",
    pond: "à¤¤à¤¾à¤²à¤¾à¤¬ (à¤¸à¤¿à¤®à¥à¤ªà¤¸à¤¨ à¤¨à¤¿à¤¯à¤®)",
    poly: "à¤¬à¤¹à¥à¤­à¥à¤œ",
    calc: "à¤•à¥à¤·à¥‡à¤¤à¥à¤°à¤«à¤² à¤•à¥€ à¤—à¤£à¤¨à¤¾ à¤•à¤°à¥‡à¤‚",
    calcDiagonal: "à¤µà¤¿à¤•à¤°à¥à¤£ à¤•à¥‡ à¤¸à¤¾à¤¥",
    calcPatwari: "à¤ªà¤Ÿà¤µà¤¾à¤°à¥€ à¤…à¤¨à¥à¤®à¤¾à¤¨",
    reset: "à¤°à¥€à¤¸à¥‡à¤Ÿ à¤•à¤°à¥‡à¤‚",
    print: "à¤ªà¥à¤°à¤¿à¤‚à¤Ÿ à¤•à¤°à¥‡à¤‚",
    history: "à¤‡à¤¤à¤¿à¤¹à¤¾à¤¸",
    clearHistory: "à¤‡à¤¤à¤¿à¤¹à¤¾à¤¸ à¤¸à¤¾à¤« à¤•à¤°à¥‡à¤‚",
    calculationN: "à¤—à¤£à¤¨à¤¾ {n} ({time})",
    addPoint: "à¤¬à¤¿à¤‚à¤¦à¥ à¤œà¥‹à¤¡à¤¼à¥‡à¤‚",
    removePoint: "à¤¬à¤¿à¤‚à¤¦à¥ à¤¹à¤Ÿà¤¾à¤à¤",
    addSide: "à¤­à¥à¤œà¤¾ à¤œà¥‹à¤¡à¤¼à¥‡à¤‚",
    removeSide: "à¤­à¥à¤œà¤¾ à¤¹à¤Ÿà¤¾à¤à¤",
    dropdownPlaceholder: "à¤…à¤§à¤¿à¤• à¤†à¤•à¤¾à¤°",
    rectLength: "à¤²à¤‚à¤¬à¤¾à¤ˆ (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    rectWidth: "à¤šà¥Œà¤¡à¤¼à¤¾à¤ˆ (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    rectShare: "à¤¹à¤¿à¤¸à¥à¤¸à¤¾ (à¤‰à¤¦à¤¾à¤¹à¤°à¤£: 1 à¤¯à¤¾ 3/4)",
    quadNorth: "à¤‰à¤¤à¥à¤¤à¤° à¤­à¥à¤œà¤¾ (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    quadEast: "à¤ªà¥‚à¤°à¥à¤µ à¤­à¥à¤œà¤¾ (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    quadSouth: "à¤¦à¤•à¥à¤·à¤¿à¤£ à¤­à¥à¤œà¤¾ (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    quadWest: "à¤ªà¤¶à¥à¤šà¤¿à¤® à¤­à¥à¤œà¤¾ (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    quadDiag: "à¤µà¤¿à¤•à¤°à¥à¤£ (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    irregShare: "à¤¹à¤¿à¤¸à¥à¤¸à¤¾ (à¤‰à¤¦à¤¾à¤¹à¤°à¤£: 1 à¤¯à¤¾ 2/3)",
    triT1: "à¤­à¥à¤œà¤¾ 1 (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    triT2: "à¤­à¥à¤œà¤¾ 2 (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    triT3: "à¤­à¥à¤œà¤¾ 3 (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    triShare: "à¤¹à¤¿à¤¸à¥à¤¸à¤¾ (à¤‰à¤¦à¤¾à¤¹à¤°à¤£: 1 à¤¯à¤¾ 3/4)",
    pentaAB: "à¤­à¥à¤œà¤¾ AB (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    pentaBC: "à¤­à¥à¤œà¤¾ BC (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    pentaCD: "à¤­à¥à¤œà¤¾ CD (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    pentaDE: "à¤­à¥à¤œà¤¾ DE (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    pentaEA: "à¤­à¥à¤œà¤¾ EA (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    pentaD1: "à¤µà¤¿à¤•à¤°à¥à¤£ 1 (à¤‰à¤¦à¤¾à¤¹à¤°à¤£: AC) (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    pentaD2: "à¤µà¤¿à¤•à¤°à¥à¤£ 2 (à¤‰à¤¦à¤¾à¤¹à¤°à¤£: AD) (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    pentShare: "à¤¹à¤¿à¤¸à¥à¤¸à¤¾ (à¤‰à¤¦à¤¾à¤¹à¤°à¤£: 1 à¤¯à¤¾ 1/2)",
    pondDistance: "à¤¸à¤¾à¤®à¤¾à¤¨à¥à¤¯ à¤¦à¥‚à¤°à¥€ d (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    pondShare: "à¤¹à¤¿à¤¸à¥à¤¸à¤¾ (à¤‰à¤¦à¤¾à¤¹à¤°à¤£: 1 à¤¯à¤¾ 3/4)",
    polySide: "à¤­à¥à¤œà¤¾ {side} (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    polyDiag: "à¤µà¤¿à¤•à¤°à¥à¤£ A{point} (à¤•à¤°à¤® à¤®à¥‡à¤‚)",
    polyShare: "à¤¹à¤¿à¤¸à¥à¤¸à¤¾ (à¤‰à¤¦à¤¾à¤¹à¤°à¤£: 1 à¤¯à¤¾ 3/4)",
    patwariTrapeziumNote: "à¤ªà¤Ÿà¤µà¤¾à¤°à¥€ à¤…à¤¨à¥à¤®à¤¾à¤¨: à¤¸à¤®à¤²à¤‚à¤¬ à¤šà¤¤à¥à¤°à¥à¤­à¥à¤œ à¤•à¤¾ à¤‰à¤ªà¤¯à¥‹à¤— à¤•à¤¿à¤¯à¤¾ à¤—à¤¯à¤¾",
    errorPatwariLargeDiff: "à¤µà¤¿à¤ªà¤°à¥€à¤¤ à¤­à¥à¤œà¤¾à¤“à¤‚ à¤•à¤¾ à¤…à¤‚à¤¤à¤° 5 à¤¯à¤¾ à¤…à¤§à¤¿à¤• à¤•à¤°à¤® à¤¹à¥ˆà¥¤ à¤•à¥ƒà¤ªà¤¯à¤¾ à¤¸à¤Ÿà¥€à¤• à¤—à¤£à¤¨à¤¾ à¤•à¥‡ à¤²à¤¿à¤ 'à¤µà¤¿à¤•à¤°à¥à¤£ à¤•à¥‡ à¤¸à¤¾à¤¥' à¤¬à¤Ÿà¤¨ à¤•à¤¾ à¤‰à¤ªà¤¯à¥‹à¤— à¤•à¤°à¥‡à¤‚à¥¤",
    totalArea: "à¤•à¥à¤² à¤•à¥à¤·à¥‡à¤¤à¥à¤°à¤«à¤²",
    shareArea: "à¤¹à¤¿à¤¸à¥à¤¸à¥‡ à¤•à¥‡ à¤…à¤¨à¥à¤¸à¤¾à¤° à¤•à¥à¤·à¥‡à¤¤à¥à¤°à¤«à¤²",
    errorLength: "à¤²à¤‚à¤¬à¤¾à¤ˆ à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorWidth: "à¤šà¥Œà¤¡à¤¼à¤¾à¤ˆ à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorShare: "à¤…à¤®à¤¾à¤¨à¥à¤¯ à¤¹à¤¿à¤¸à¥à¤¸à¤¾ à¤ªà¥à¤°à¤¾à¤°à¥‚à¤ªà¥¤",
    errorSideNorth: "à¤‰à¤¤à¥à¤¤à¤° à¤­à¥à¤œà¤¾ à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorSideEast: "à¤ªà¥‚à¤°à¥à¤µ à¤­à¥à¤œà¤¾ à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorSideSouth: "à¤¦à¤•à¥à¤·à¤¿à¤£ à¤­à¥à¤œà¤¾ à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorSideWest: "à¤ªà¤¶à¥à¤šà¤¿à¤® à¤­à¥à¤œà¤¾ à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorDiagonal: "à¤µà¤¿à¤•à¤°à¥à¤£ à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorSide1: "à¤­à¥à¤œà¤¾ 1 à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorSide2: "à¤­à¥à¤œà¤¾ 2 à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorSide3: "à¤­à¥à¤œà¤¾ 3 à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorSideAB: "à¤­à¥à¤œà¤¾ AB à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorSideBC: "à¤­à¥à¤œà¤¾ BC à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorSideCD: "à¤­à¥à¤œà¤¾ CD à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorSideDE: "à¤­à¥à¤œà¤¾ DE à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorSideEA: "à¤­à¥à¤œà¤¾ EA à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorDiagonal1: "à¤µà¤¿à¤•à¤°à¥à¤£ 1 à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorDiagonal2: "à¤µà¤¿à¤•à¤°à¥à¤£ 2 à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorDistance: "à¤¸à¤¾à¤®à¤¾à¤¨à¥à¤¯ à¤¦à¥‚à¤°à¥€ à¤à¤• à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorPondPoints: "à¤¤à¤¾à¤²à¤¾à¤¬ à¤—à¤£à¤¨à¤¾ à¤•à¥‡ à¤²à¤¿à¤ à¤•à¤® à¤¸à¥‡ à¤•à¤® 6 à¤¬à¤¿à¤‚à¤¦à¥ à¤”à¤° à¤¸à¤® à¤¸à¤‚à¤–à¥à¤¯à¤¾ à¤®à¥‡à¤‚ à¤¬à¤¿à¤‚à¤¦à¥ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorPondHeights: "à¤¸à¤­à¥€ à¤¤à¤¾à¤²à¤¾à¤¬ à¤Šà¤‚à¤šà¤¾à¤‡à¤¯à¤¾à¤ à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤¹à¥‹à¤¨à¥€ à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorPolySides: "à¤¬à¤¹à¥à¤­à¥à¤œ à¤•à¥‡ à¤²à¤¿à¤ à¤•à¤® à¤¸à¥‡ à¤•à¤® 6 à¤­à¥à¤œà¤¾à¤à¤ à¤”à¤° à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorPolyDiags: "à¤¬à¤¹à¥à¤­à¥à¤œ à¤•à¥‡ à¤²à¤¿à¤ à¤•à¤® à¤¸à¥‡ à¤•à¤® {n} à¤µà¤¿à¤•à¤°à¥à¤£ à¤”à¤° à¤§à¤¨à¤¾à¤¤à¥à¤®à¤• à¤ªà¥‚à¤°à¥à¤£à¤¾à¤‚à¤• à¤šà¤¾à¤¹à¤¿à¤à¥¤",
    errorInvalidTriangle: "à¤­à¥à¤œà¤¾à¤“à¤‚ à¤¦à¥à¤µà¤¾à¤°à¤¾ à¤…à¤®à¤¾à¤¨à¥à¤¯ à¤¤à¥à¤°à¤¿à¤•à¥‹à¤£ à¤¬à¤¨à¤¾à¥¤",
    errorInvalidTriangles: "à¤­à¥à¤œà¤¾à¤“à¤‚ à¤”à¤° à¤µà¤¿à¤•à¤°à¥à¤£à¥‹à¤‚ à¤¦à¥à¤µà¤¾à¤°à¤¾ à¤…à¤®à¤¾à¤¨à¥à¤¯ à¤¤à¥à¤°à¤¿à¤•à¥‹à¤£ à¤¬à¤¨à¥‡à¥¤",
    errorInvalidTrapezium: "à¤…à¤®à¤¾à¤¨à¥à¤¯ à¤¸à¤®à¤²à¤‚à¤¬ à¤šà¤¤à¥à¤°à¥à¤­à¥à¤œ à¤¬à¤¨à¤¾à¥¤",
    errorGeneral: "à¤•à¥ƒà¤ªà¤¯à¤¾ à¤šà¤¯à¤¨à¤¿à¤¤ à¤†à¤•à¤¾à¤° à¤•à¥‡ à¤²à¤¿à¤ à¤®à¤¾à¤¨à¥à¤¯ à¤‡à¤¨à¤ªà¥à¤Ÿ à¤ªà¥à¤°à¤¦à¤¾à¤¨ à¤•à¤°à¥‡à¤‚à¥¤"
  }
};

/* --- Utility Functions --- */
function resetForm() {
  document.querySelectorAll('input').forEach(i => i.value = '');
  domCache.result.style.display = 'none';
  domCache.result.classList.remove('error');
  domCache.printBtn.style.display = 'none';
  domCache.shapeDropdown.classList.remove('active');
  domCache.shapeDropdown.selectedIndex = 0;
  pondIndex = 0;
  polySideIndex = 0;
  document.getElementById('pondPointsContainer').innerHTML = '';
  document.getElementById('polySidesContainer').innerHTML = '';
  document.getElementById('polyDiagsContainer').innerHTML = '';
  if (domCache.pondSection.classList.contains('visible')) {
    initializePondPoints();
  }
  if (domCache.polySection.classList.contains('visible')) {
    initializePolySides();
  }
  clearResultCanvas();
  domCache.historyList.style.display = 'none';
}

const sections = {
  rect: 'rectSection',
  quad: 'quadSection',
  tri: 'triSection',
  penta: 'pentaSection',
  pond: 'pondSection',
  poly: 'polySection'
};

function selectShape(shape) {
  resetForm();
  for (let key in sections) {
    document.getElementById(sections[key]).classList.remove('visible');
    const btn = document.getElementById('btn-' + key);
    if (btn) btn.classList.remove('active');
  }
  if (shape === 'rect' || shape === 'quad') {
    document.getElementById('btn-' + shape).classList.add('active');
  }
  document.getElementById(sections[shape]).classList.add('visible');
  domCache.quadCalcButtons.classList.toggle('visible', shape === 'quad');
  if (shape === 'quad') {
    selectQuadMethod('diagonal');
  }
  if (shape === 'pond') {
    initializePondPoints();
  }
  if (shape === 'poly') {
    initializePolySides();
  }
  resizeCanvas();
}

function handleDropdownShape(select) {
  const shape = select.value;
  if (shape) {
    selectShape(shape);
    domCache.shapeDropdown.classList.add('active');
    // Update the dropdown's displayed text to the selected shape
    const selectedOption = select.options[select.selectedIndex].text;
    select.options[0].text = selectedOption;
    select.value = shape; // Ensure the correct value is maintained
  } else {
    domCache.shapeDropdown.classList.remove('active');
    select.options[0].text = translations[document.getElementById('language').value].dropdownPlaceholder;
  }
}
function parseShare(value) {
  if (!value || value.trim() === '') return 1;
  if (value.includes('/')) {
    const parts = value.split('/');
    const num = parseFloat(parts[0]);
    const denom = parseFloat(parts[1]);
    if (isNaN(num) || isNaN(denom) || denom === 0) return NaN;
    return num / denom;
  }
  return parseFloat(value);
}

function isValidInput(value) {
  return !isNaN(value) && value > 0 && Number.isInteger(value);
}

function isValidTriangle(a, b, c) {
  return isValidInput(a) && isValidInput(b) && isValidInput(c) &&
         a + b > c && b + c > a && a + c > b;
}

function triangleArea(a, b, c) {
  if (!isValidTriangle(a, b, c)) return NaN;
  const s = (a + b + c) / 2;
  return Math.sqrt(s * (s - a) * (s - b) * (s - c));
}

/* --- Area Conversion --- */
function convertSqftToAcm(sqft) {
  const lang = document.getElementById('language').value;
  const acreSqFt = 43560;
  const kanalSqFt = 5445;
  const marlaSqFt = 272.25;
  const sarsaiSqFt = marlaSqFt / 9;

  let acres = Math.floor(sqft / acreSqFt);
  let rem = sqft - acres * acreSqFt;
  let kanal = Math.floor(rem / kanalSqFt);
  rem = rem - kanal * kanalSqFt;
  let marla = Math.floor(rem / marlaSqFt);
  rem = rem - marla * marlaSqFt;
  let sarsai = Math.round(rem / sarsaiSqFt);

  if (sarsai >= 9) {
    marla += Math.floor(sarsai / 9);
    sarsai = sarsai % 9;
  }
  if (marla >= 20) {
    kanal += Math.floor(marla / 20);
    marla = marla % 20;
  }
  if (kanal >= 8) {
    acres += Math.floor(kanal / 8);
    kanal = kanal % 8;
  }

  const marlaLabel = lang === 'hi' ? 'à¤®à¤°à¤²à¤¾' : 'marla';
  return `${acres} acre ${kanal} kanal ${marla} ${marlaLabel} ${sarsai} sarsai`;
}

/* --- Pond Section --- */
let pondIndex = 0;

function initializePondPoints() {
  pondIndex = 0;
  document.getElementById('pondPointsContainer').innerHTML = '';
  for (let i = 0; i < 6; i++) {
    addPondPoint();
  }
}

function addPondPoint() {
  pondIndex++;
  const div = document.createElement('div');
  div.className = 'pond-point';
  div.innerHTML = `<label>P${pondIndex}</label><input type="number" min="1" class="pond-height" />`;
  document.getElementById('pondPointsContainer').appendChild(div);
}

function removePondPoint() {
  const container = document.getElementById('pondPointsContainer');
  if (pondIndex > 6) {
    container.removeChild(container.lastChild);
    pondIndex--;
  }
}

function validatePondPoints() {
  const heights = document.querySelectorAll('.pond-height').length;
  return heights >= 6 && heights % 2 === 0;
}

/* --- Polygon Section --- */
let polySideIndex = 0;

function initializePolySides() {
  polySideIndex = 0;
  document.getElementById('polySidesContainer').innerHTML = '';
  document.getElementById('polyDiagsContainer').innerHTML = '';
  for (let i = 0; i < 6; i++) {
    addPolySide();
  }
}

function addPolySide() {
  if (polySideIndex >= 25) return; // Limit to 25 sides (A to Y)
  polySideIndex++;
  const container = document.getElementById('polySidesContainer');
  const div = document.createElement('div');
  div.className = 'poly-side';
  const sideLabel = String.fromCharCode(65 + polySideIndex - 1) + String.fromCharCode(65 + (polySideIndex % 26));
  const lang = document.getElementById('language').value;
  div.innerHTML = `<label>${translations[lang].polySide.replace('{side}', sideLabel)}</label><input type="number" min="1" class="poly-side-input" />`;
  container.appendChild(div);
  updatePolyDiags();
}

function removePolySide() {
  const container = document.getElementById('polySidesContainer');
  if (polySideIndex > 6) {
    container.removeChild(container.lastChild);
    polySideIndex--;
    updatePolyDiags();
  }
}

function updatePolyDiags() {
  const container = document.getElementById('polyDiagsContainer');
  container.innerHTML = '';
  const lang = document.getElementById('language').value;
  const requiredDiags = Math.max(0, polySideIndex - 3);
  for (let i = 0; i < requiredDiags; i++) {
    const div = document.createElement('div');
    div.className = 'poly-diag';
    const diagLabel = 'A' + String.fromCharCode(67 + i);
    div.innerHTML = `<label>${translations[lang].polyDiag.replace('{point}', diagLabel[1])}</label><input type="number" min="1" class="poly-diag-input" />`;
    container.appendChild(div);
  }
}

function validatePolyInputs() {
  const sides = document.querySelectorAll('.poly-side-input').length;
  const diags = document.querySelectorAll('.poly-diag-input').length;
  const requiredDiags = Math.max(0, sides - 3);
  return sides >= 6 && diags >= requiredDiags;
}

/* --- Quadrilateral Calculation --- */
const KARAM_TO_FT2 = 30.25;
let quadMethod = 'diagonal';

function selectQuadMethod(method) {
  quadMethod = method;
  const diagBtn = document.getElementById('calcDiagBtn');
  const patwariBtn = document.getElementById('calcPatwariBtn');
  const diagWrapper = document.getElementById('diagWrapper');
  if (method === 'diagonal') {
    diagBtn.classList.add('active');
    patwariBtn.classList.remove('active');
    diagWrapper.classList.remove('hidden');
  } else {
    patwariBtn.classList.add('active');
    diagBtn.classList.remove('active');
    diagWrapper.classList.add('hidden');
    document.getElementById('diag').value = '';
  }
}

/* --- Canvas for Result Figure --- */
function resizeCanvas() {
  const isDesktop = window.innerWidth >= 768;
  if (isDesktop) {
    domCache.resultCanvas.width = 600;
    domCache.resultCanvas.height = 400;
  } else {
    domCache.resultCanvas.width = window.innerWidth - 40;
    domCache.resultCanvas.height = 300;
  }
}

function clearResultCanvas() {
  domCache.resultCanvas.style.display = 'none';
  const ctx = domCache.resultCanvas.getContext('2d');
  ctx.clearRect(0, 0, domCache.resultCanvas.width, domCache.resultCanvas.height);
}

function drawResultFigure(shape, inputs, areaText) {
  const ctx = domCache.resultCanvas.getContext('2d');
  clearResultCanvas();
  domCache.resultCanvas.style.display = 'block';
  ctx.font = '12px Poppins';
  ctx.textAlign = 'center';
  ctx.strokeStyle = '#000';
  ctx.fillStyle = '#000';
  const scale = 12;
  const centerX = domCache.resultCanvas.width / 2;
  const centerY = domCache.resultCanvas.height / 2;
  const lang = document.getElementById('language').value;
  const unit = lang === 'hi' ? 'à¤•à¤°à¤®' : 'karam';

  if (shape === 'rect') {
    const l = (inputs.rectLength || 10) * scale;
    const w = (inputs.rectWidth || 10) * scale;
    const x = centerX - l / 2;
    const y = centerY - w / 2;
    ctx.strokeRect(x, y, l, w);
    ctx.fillText(`${inputs.rectLength || 'L'} ${unit}`, centerX, y - 10);
    ctx.fillText(`${inputs.rectWidth || 'W'} ${unit}`, x - 20, centerY);
    ctx.font = '14px Poppins';
    ctx.fillText(areaText, centerX, domCache.resultCanvas.height - 20);
  }
  else if (shape === 'tri') {
    const a = (inputs.t1 || 10) * scale;
    const b = (inputs.t2 || 10) * scale;
    const c = (inputs.t3 || 10) * scale;
    ctx.beginPath();
    ctx.moveTo(centerX - a / 2, centerY + 50);
    ctx.lineTo(centerX + a / 2, centerY + 50);
    ctx.lineTo(centerX, centerY - 50);
    ctx.closePath();
    ctx.stroke();
    ctx.fillText(`${inputs.t1 || 'S1'} ${unit}`, centerX, centerY + 70);
    ctx.fillText(`${inputs.t2 || 'S2'} ${unit}`, centerX + a / 2 + 20, centerY);
    ctx.fillText(`${inputs.t3 || 'S3'} ${unit}`, centerX - a / 2 - 20, centerY);
    ctx.font = '14px Poppins';
    ctx.fillText(areaText, centerX, domCache.resultCanvas.height - 20);
  }
  else if (shape === 'quad') {
    const n = (inputs.north || 10) * scale;
    const e = (inputs.east || 10) * scale;
    const s = (inputs.south || 10) * scale;
    const w = (inputs.west || 10) * scale;
    const vertices = [
      {x: centerX - n / 2, y: centerY - 50},
      {x: centerX + n / 2, y: centerY - 50},
      {x: centerX + n / 2 - (n - s) / 2, y: centerY + 50},
      {x: centerX - n / 2 + (n - s) / 2, y: centerY + 50}
    ];
    ctx.beginPath();
    ctx.moveTo(vertices[0].x, vertices[0].y);
    for (let i = 1; i < vertices.length; i++) {
      ctx.lineTo(vertices[i].x, vertices[i].y);
    }
    ctx.closePath();
    ctx.stroke();
    ctx.fillText(`${inputs.north || 'N'} ${unit}`, centerX, vertices[0].y - 10);
    ctx.fillText(`${inputs.east || 'E'} ${unit}`, vertices[1].x + 20, centerY);
    ctx.fillText(`${inputs.south || 'S'} ${unit}`, centerX, vertices[2].y + 20);
    ctx.fillText(`${inputs.west || 'W'} ${unit}`, vertices[3].x - 20, centerY);
    if (quadMethod === 'diagonal' && inputs.diag) {
      ctx.beginPath();
      ctx.moveTo(vertices[0].x, vertices[0].y);
      ctx.lineTo(vertices[2].x, vertices[2].y);
      ctx.strokeStyle = '#888';
      ctx.stroke();
      ctx.strokeStyle = '#000';
      ctx.fillText(`${inputs.diag || 'D'} ${unit}`, (vertices[0].x + vertices[2].x) / 2, (vertices[0].y + vertices[2].y) / 2);
    }
    ctx.font = '14px Poppins';
    ctx.fillText(areaText, centerX, domCache.resultCanvas.height - 20);
  }
  else if (shape === 'penta') {
    const sides = ['ab', 'bc', 'cd', 'de', 'ea'].map(id => (inputs[id] || 10) * scale);
    const diags = ['d1', 'd2'].map(id => (inputs[id] || 10) * scale);
    const r = Math.max(...sides, ...diags) / 2;
    const vertices = [];
    for (let i = 0; i < 5; i++) {
      const angle = (i * 2 * Math.PI / 5) - Math.PI / 2;
      vertices.push({
        x: centerX + r * Math.cos(angle),
        y: centerY + r * Math.sin(angle)
      });
    }
    ctx.beginPath();
    ctx.moveTo(vertices[0].x, vertices[0].y);
    for (let i = 1; i < vertices.length; i++) {
      ctx.lineTo(vertices[i].x, vertices[i].y);
    }
    ctx.closePath();
    ctx.stroke();
    ctx.fillText(`${inputs.ab || 'AB'} ${unit}`, (vertices[0].x + vertices[1].x) / 2, vertices[0].y - 20);
    ctx.fillText(`${inputs.bc || 'BC'} ${unit}`, vertices[1].x + 20, (vertices[1].y + vertices[2].y) / 2);
    ctx.fillText(`${inputs.cd || 'CD'} ${unit}`, (vertices[2].x + vertices[3].x) / 2, vertices[2].y + 20);
    ctx.fillText(`${inputs.de || 'DE'} ${unit}`, vertices[3].x - 20, (vertices[3].y + vertices[4].y) / 2);
    ctx.fillText(`${inputs.ea || 'EA'} ${unit}`, (vertices[4].x + vertices[0].x) / 2, vertices[4].y + 20);
    if (inputs.d1) {
      ctx.beginPath();
      ctx.moveTo(vertices[0].x, vertices[0].y);
      ctx.lineTo(vertices[2].x, vertices[2].y);
      ctx.strokeStyle = '#888';
      ctx.stroke();
      ctx.strokeStyle = '#000';
      ctx.fillText(`${inputs.d1 || 'AC'} ${unit}`, (vertices[0].x + vertices[2].x) / 2, (vertices[0].y + vertices[2].y) / 2);
    }
    if (inputs.d2) {
      ctx.beginPath();
      ctx.moveTo(vertices[0].x, vertices[0].y);
      ctx.lineTo(vertices[3].x, vertices[3].y);
      ctx.strokeStyle = '#888';
      ctx.stroke();
      ctx.strokeStyle = '#000';
      ctx.fillText(`${inputs.d2 || 'AD'} ${unit}`, (vertices[0].x + vertices[3].x) / 2, (vertices[0].y + vertices[3].y) / 2);
    }
    ctx.font = '14px Poppins';
    ctx.fillText(areaText, centerX, domCache.resultCanvas.height - 20);
  }
  else if (shape === 'poly') {
    const sides = inputs.sides.map(s => (s || 10) * scale);
    const diags = inputs.diags.map(d => (d || 10) * scale);
    const n = sides.length;
    const r = Math.max(...sides, ...diags, 50) / 2;
    const vertices = [];
    for (let i = 0; i < n; i++) {
      const angle = (i * 2 * Math.PI / n) - Math.PI / 2;
      vertices.push({
        x: centerX + r * Math.cos(angle),
        y: centerY + r * Math.sin(angle)
      });
    }
    ctx.beginPath();
    ctx.moveTo(vertices[0].x, vertices[0].y);
    for (let i = 1; i < n; i++) {
      ctx.lineTo(vertices[i].x, vertices[i].y);
    }
    ctx.closePath();
    ctx.stroke();
    for (let i = 0; i < n; i++) {
      const next = (i + 1) % n;
      ctx.fillText(
        `${inputs.sides[i] || String.fromCharCode(65+i)+'-'+String.fromCharCode(65+(i+1)%n)} ${unit}`,
        (vertices[i].x + vertices[next].x) / 2,
        (vertices[i].y + vertices[next].y) / 2
      );
    }
    for (let i = 0; i < diags.length; i++) {
      ctx.beginPath();
      ctx.moveTo(vertices[0].x, vertices[0].y);
      ctx.lineTo(vertices[i + 2].x, vertices[i + 2].y);
      ctx.strokeStyle = '#888';
      ctx.stroke();
      ctx.strokeStyle = '#000';
      ctx.fillText(
        `A${String.fromCharCode(67+i)}: ${inputs.diags[i] || ''} ${unit}`,
        (vertices[0].x + vertices[i + 2].x) / 2,
        (vertices[0].y + vertices[i + 2].y) / 2
      );
    }
    ctx.font = '14px Poppins';
    ctx.fillText(areaText, centerX, domCache.resultCanvas.height - 20);
  }
  else if (shape === 'pond') {
    const heights = inputs.heights.map(h => (h || 10) * scale);
    const d = (inputs.distance || 10) * scale;
    const totalWidth = (heights.length - 1) * d;
    const startX = centerX - totalWidth / 2;
    const baseY = centerY + 50;
    ctx.beginPath();
    ctx.setLineDash([5, 5]);
    ctx.moveTo(startX, baseY);
    ctx.lineTo(startX + totalWidth, baseY);
    ctx.strokeStyle = '#888';
    ctx.stroke();
    ctx.setLineDash([]);
    ctx.strokeStyle = '#000';
    ctx.beginPath();
    for (let i = 0; i < heights.length; i++) {
      const x = startX + i * d;
      const y = baseY - heights[i];
      if (i === 0) ctx.moveTo(x, y);
      else ctx.lineTo(x, y);
    }
    ctx.stroke();
    for (let i = 0; i < heights.length; i++) {
      const x = startX + i * d;
      ctx.fillText(`P${i+1}: ${inputs.heights[i] || 'H'} ${unit}`, x, baseY + 30);
    }
    ctx.fillText(`d: ${inputs.distance || 'd'} ${unit}`, centerX, baseY + 50);
    ctx.font = '14px Poppins';
    ctx.fillText(areaText, centerX, domCache.resultCanvas.height - 20);
  }
}

/* --- History Management --- */
function saveCalculation(shape, inputs, result) {
  const data = { inputs, result, shape, timestamp: new Date().toISOString() };
  let history = JSON.parse(localStorage.getItem('history') || '[]');
  history.unshift(data);
  if (history.length > 10) history.pop();
  localStorage.setItem('history', JSON.stringify(history));
  domCache.historyBtn.disabled = false;
  updateHistoryList();
}

function toggleHistory() {
  const isVisible = domCache.historyList.style.display === 'block';
  domCache.historyList.style.display = isVisible ? 'none' : 'block';
  if (!isVisible) updateHistoryList();
}

function updateHistoryList() {
  const history = JSON.parse(localStorage.getItem('history') || '[]');
  const lang = document.getElementById('language').value;
  domCache.historyList.innerHTML = '';
  history.forEach((item, index) => {
    const btn = document.createElement('button');
    btn.textContent = translations[lang].calculationN
      .replace('{n}', index + 1)
      .replace('{time}', new Date(item.timestamp).toLocaleString());
    btn.onclick = () => loadHistoryItem(item);
    domCache.historyList.appendChild(btn);
  });
  // Re-append the Clear History button
  const clearBtn = document.createElement('button');
  clearBtn.id = 'clearHistoryBtn';
  clearBtn.textContent = translations[lang].clearHistory;
  clearBtn.setAttribute('aria-label', 'Clear Calculation History');
  clearBtn.onclick = clearHistory;
  domCache.historyList.appendChild(clearBtn);
}

function clearHistory() {
  localStorage.removeItem('history');
  domCache.historyList.innerHTML = '';
  domCache.historyList.style.display = 'none';
  domCache.historyBtn.disabled = true;
}

function loadHistoryItem(item) {
  selectShape(item.shape);
  for (let [id, value] of Object.entries(item.inputs)) {
    const input = document.getElementById(id);
    if (input) input.value = value;
  }
  if (item.shape === 'poly') {
    initializePolySides();
    const sideInputs = document.querySelectorAll('.poly-side-input');
    item.inputs.sides.forEach((val, i) => {
      if (sideInputs[i]) sideInputs[i].value = val;
    });
    const diagInputs = document.querySelectorAll('.poly-diag-input');
    item.inputs.diags.forEach((val, i) => {
      if (diagInputs[i]) diagInputs[i].value = val;
    });
  }
  if (item.shape === 'pond') {
    initializePondPoints();
    const heightInputs = document.querySelectorAll('.pond-height');
    item.inputs.heights.forEach((val, i) => {
      if (heightInputs[i]) heightInputs[i].value = val;
    });
    while (heightInputs.length < item.inputs.heights.length) {
      addPondPoint();
      document.querySelectorAll('.pond-height')[heightInputs.length].value = item.inputs.heights[heightInputs.length];
    }
  }
  if (item.shape === 'quad' && item.inputs.method) {
    selectQuadMethod(item.inputs.method);
  }
  domCache.result.innerText = item.result;
  domCache.result.style.display = 'block';
  domCache.result.classList.remove('error');
  domCache.printBtn.style.display = 'block';
  drawResultFigure(item.shape, item.inputs, item.result.split('\n')[2]?.replace('â€¢ ', '') || '');
  domCache.historyList.style.display = 'none';
}

/* --- Main Calculation --- */
function calculate() {
  let rawAreaKaram2 = 0;
  let shareAreaKaram2 = 0;
  let error = '';
  const lang = document.getElementById('language').value;
  const shape = Object.keys(sections).find(key => document.getElementById(sections[key]).classList.contains('visible'));
  const inputs = {};

  if (domCache.rectSection.classList.contains('visible')) {
    const l = parseInt(document.getElementById('rectLength').value);
    const w = parseInt(document.getElementById('rectWidth').value);
    const share = parseShare(document.getElementById('rectShare').value);
    inputs.rectLength = l;
    inputs.rectWidth = w;
    if (!isValidInput(l)) error = translations[lang].errorLength;
    else if (!isValidInput(w)) error = translations[lang].errorWidth;
    else if (isNaN(share)) error = translations[lang].errorShare;
    else {
      rawAreaKaram2 = l * w;
      shareAreaKaram2 = rawAreaKaram2 * share;
    }
  }
  else if (domCache.quadSection.classList.contains('visible')) {
    const north = parseInt(document.getElementById('north').value);
    const east = parseInt(document.getElementById('east').value);
    const south = parseInt(document.getElementById('south').value);
    const west = parseInt(document.getElementById('west').value);
    const diag = parseInt(document.getElementById('diag').value);
    const share = parseShare(document.getElementById('irregShare').value);
    inputs.north = north;
    inputs.east = east;
    inputs.south = south;
    inputs.west = west;
    inputs.diag = diag;
    inputs.method = quadMethod;
    if (!isValidInput(north)) error = translations[lang].errorSideNorth;
    else if (!isValidInput(east)) error = translations[lang].errorSideEast;
    else if (!isValidInput(south)) error = translations[lang].errorSideSouth;
    else if (!isValidInput(west)) error = translations[lang].errorSideWest;
    else if (isNaN(share)) error = translations[lang].errorShare;
    else if (quadMethod === 'diagonal') {
      if (!isValidInput(diag)) error = translations[lang].errorDiagonal;
      else {
        const tri1 = triangleArea(north, east, diag);
        const tri2 = triangleArea(south, west, diag);
        if (isNaN(tri1) || isNaN(tri2)) {
          error = translations[lang].errorInvalidTriangle;
        } else {
          rawAreaKaram2 = tri1 + tri2;
          shareAreaKaram2 = rawAreaKaram2 * share;
        }
      }
    } else if (quadMethod === 'patwari') {
      const diffNS = Math.abs(north - south);
      const diffEW = Math.abs(east - west);
      if (diffNS > 5 || diffEW > 5) {
        error = translations[lang].errorPatwariLargeDiff;
      } else {
        if (diffNS <= diffEW) {
          rawAreaKaram2 = ((north + south) * east) / 2;
        } else {
          rawAreaKaram2 = ((east + west) * north) / 2;
        }
        if (isNaN(rawAreaKaram2) || rawAreaKaram2 <= 0) {
          error = translations[lang].errorInvalidTrapezium;
        } else {
          shareAreaKaram2 = rawAreaKaram2 * share;
        }
      }
    }
  }
  else if (domCache.triSection.classList.contains('visible')) {
    const a = parseInt(document.getElementById('t1').value);
    const b = parseInt(document.getElementById('t2').value);
    const c = parseInt(document.getElementById('t3').value);
    const share = parseShare(document.getElementById('triShare').value);
    inputs.t1 = a;
    inputs.t2 = b;
    inputs.t3 = c;
    if (!isValidInput(a)) error = translations[lang].errorSide1;
    else if (!isValidInput(b)) error = translations[lang].errorSide2;
    else if (!isValidInput(c)) error = translations[lang].errorSide3;
    else if (isNaN(share)) error = translations[lang].errorShare;
    else {
      rawAreaKaram2 = triangleArea(a, b, c);
      if (isNaN(rawAreaKaram2)) error = translations[lang].errorInvalidTriangle;
      else shareAreaKaram2 = rawAreaKaram2 * share;
    }
  }
  else if (domCache.pentaSection.classList.contains('visible')) {
    const ab = parseInt(document.getElementById('ab').value);
    const bc = parseInt(document.getElementById('bc').value);
    const cd = parseInt(document.getElementById('cd').value);
    const de = parseInt(document.getElementById('de').value);
    const ea = parseInt(document.getElementById('ea').value);
    const d1 = parseInt(document.getElementById('d1').value);
    const d2 = parseInt(document.getElementById('d2').value);
    const share = parseShare(document.getElementById('pentShare').value);
    inputs.ab = ab;
    inputs.bc = bc;
    inputs.cd = cd;
    inputs.de = de;
    inputs.ea = ea;
    inputs.d1 = d1;
    inputs.d2 = d2;
    if (!isValidInput(ab)) error = translations[lang].errorSideAB;
    else if (!isValidInput(bc)) error = translations[lang].errorSideBC;
    else if (!isValidInput(cd)) error = translations[lang].errorSideCD;
    else if (!isValidInput(de)) error = translations[lang].errorSideDE;
    else if (!isValidInput(ea)) error = translations[lang].errorSideEA;
    else if (!isValidInput(d1)) error = translations[lang].errorDiagonal1;
    else if (!isValidInput(d2)) error = translations[lang].errorDiagonal2;
    else if (isNaN(share)) error = translations[lang].errorShare;
    else {
      const t1 = triangleArea(ab, bc, d1);
      const t2 = triangleArea(d1, cd, d2);
      const t3 = triangleArea(d2, de, ea);
      if (isNaN(t1) || isNaN(t2) || isNaN(t3)) error = translations[lang].errorInvalidTriangles;
      else {
        rawAreaKaram2 = t1 + t2 + t3;
        shareAreaKaram2 = rawAreaKaram2 * share;
      }
    }
  }
  else if (domCache.pondSection.classList.contains('visible')) {
    const d = parseInt(document.getElementById('distance').value);
    const share = parseShare(document.getElementById('pondShare').value);
    const heights = Array.from(document.querySelectorAll('.pond-height'))
      .map(e => parseInt(e.value))
      .filter(v => !isNaN(v));
    inputs.distance = d;
    inputs.heights = heights;
    if (!isValidInput(d)) error = translations[lang].errorDistance;
    else if (isNaN(share)) error = translations[lang].errorShare;
    else if (heights.length < 6 || heights.length % 2 !== 0) {
      error = translations[lang].errorPondPoints;
    }
    else if (heights.some(h => !isValidInput(h))) {
      error = translations[lang].errorPondHeights;
    }
    else {
      let sum = heights[0] + heights[heights.length - 1];
      let oddSum = 0;
      let evenSum = 0;
      for (let i = 1; i < heights.length - 1; i++) {
        if (i % 2 === 1) oddSum += heights[i];
        else evenSum += heights[i];
      }
      sum += 4 * oddSum + 2 * evenSum;
      rawAreaKaram2 = (d / 3) * sum;
      shareAreaKaram2 = rawAreaKaram2 * share;
    }
  }
  else if (domCache.polySection.classList.contains('visible')) {
    const sides = Array.from(document.querySelectorAll('.poly-side-input')).map(e => parseInt(e.value));
    const diags = Array.from(document.querySelectorAll('.poly-diag-input')).map(e => parseInt(e.value));
    const share = parseShare(document.getElementById('polyShare').value);
    inputs.sides = sides;
    inputs.diags = diags;
    const n = sides.length;
    if (sides.length < 6) {
      error = translations[lang].errorPolySides;
    }
    else if (sides.some(s => !isValidInput(s))) {
      error = translations[lang].errorPolySides;
    }
    else if (diags.length < n - 3) {
      error = translations[lang].errorPolyDiags.replace('{n}', n - 3);
    }
    else if (diags.some(d => !isValidInput(d))) {
      error = translations[lang].errorPolyDiags.replace('{n}', n - 3);
    }
    else if (isNaN(share)) {
      error = translations[lang].errorShare;
    }
    else {
      let totalArea = 0;
      let area = triangleArea(sides[0], sides[1], diags[0]);
      if (isNaN(area)) {
        error = translations[lang].errorInvalidTriangles;
      } else {
        totalArea += area;
      }
      for (let i = 1; i < diags.length && !error; i++) {
        area = triangleArea(diags[i-1], sides[i+1], diags[i]);
        if (isNaN(area)) {
          error = translations[lang].errorInvalidTriangles;
        } else {
          totalArea += area;
        }
      }
      if (!error) {
        area = triangleArea(diags[diags.length-1], sides[sides.length-2], sides[sides.length-1]);
        if (isNaN(area)) {
          error = translations[lang].errorInvalidTriangles;
        } else {
          totalArea += area;
        }
      }
      if (!error) {
        rawAreaKaram2 = totalArea;
        shareAreaKaram2 = rawAreaKaram2 * share;
      }
    }
  }

  if (error || rawAreaKaram2 <= 0 || isNaN(rawAreaKaram2)) {
    domCache.result.innerText = error || translations[lang].errorGeneral;
    domCache.result.classList.add('error');
    domCache.result.style.display = 'block';
    domCache.printBtn.style.display = 'none';
    clearResultCanvas();
    return;
  }

  const rawAreaSqFt = rawAreaKaram2 * KARAM_TO_FT2;
  const shareAreaSqFt = shareAreaKaram2 * KARAM_TO_FT2;
  const rawAreaSqYd = rawAreaSqFt / 9;
  const shareAreaSqYd = shareAreaSqFt / 9;
  const totalAcm = convertSqftToAcm(rawAreaSqFt);
  const shareAcm = convertSqftToAcm(shareAreaSqFt);

  let output = `${translations[lang].totalArea}:\n`;
  output += `â€¢ ${rawAreaSqFt.toFixed(2)} sq ft (${rawAreaSqYd.toFixed(2)} sq yd)\n`;
  output += `â€¢ ${totalAcm}\n\n`;
  output += `${translations[lang].shareArea}:\n`;
  output += `â€¢ ${shareAreaSqFt.toFixed(2)} sq ft (${shareAreaSqYd.toFixed(2)} sq yd)\n`;
  output += `â€¢ ${shareAcm}`;
  if (shape === 'quad' && quadMethod === 'patwari') {
    output += `\n\n${translations[lang].patwariTrapeziumNote}`;
  }

  domCache.result.innerText = output;
  domCache.result.classList.remove('error');
  domCache.result.style.display = 'block';
  domCache.printBtn.style.display = 'block';

  drawResultFigure(shape, inputs, totalAcm);
  saveCalculation(shape, inputs, output);
}

/* --- Language Toggle --- */
function toggleLanguage() {
  const lang = document.getElementById('language').value;
  document.getElementById('page-title').textContent = translations[lang].title;
  document.getElementById('title').textContent = translations[lang].title;
  document.getElementById('watermark').textContent = translations[lang].watermark;
  document.getElementById('info').textContent = translations[lang].info;
  document.getElementById('btn-rect').textContent = translations[lang].rect;
  document.getElementById('btn-quad').textContent = translations[lang].quad;
  document.getElementById('calcBtn').textContent = translations[lang].calc;
  document.getElementById('calcDiagBtn').textContent = translations[lang].calcDiagonal;
  document.getElementById('calcPatwariBtn').textContent = translations[lang].calcPatwari;
  document.getElementById('resetBtn').textContent = translations[lang].reset;
  document.getElementById('printBtn').textContent = translations[lang].print;
  document.getElementById('historyBtn').textContent = translations[lang].history;
  document.getElementById('clearHistoryBtn').textContent = translations[lang].clearHistory;
  document.getElementById('addPondBtn').textContent = translations[lang].addPoint;
  document.getElementById('removePondBtn').textContent = translations[lang].removePoint;
  document.getElementById('addPolySideBtn').textContent = translations[lang].addSide;
  document.getElementById('removePolySideBtn').textContent = translations[lang].removeSide;
  domCache.shapeDropdown.options[0].text = translations[lang].dropdownPlaceholder;
  domCache.shapeDropdown.options[1].text = translations[lang].tri;
  domCache.shapeDropdown.options[2].text = translations[lang].penta;
  domCache.shapeDropdown.options[3].text = translations[lang].pond;
  domCache.shapeDropdown.options[4].text = translations[lang].poly;
  document.getElementById('rectLengthLabel').textContent = translations[lang].rectLength;
  document.getElementById('rectWidthLabel').textContent = translations[lang].rectWidth;
  document.getElementById('rectShareLabel').textContent = translations[lang].rectShare;
  document.getElementById('quadNorthLabel').textContent = translations[lang].quadNorth;
  document.getElementById('quadEastLabel').textContent = translations[lang].quadEast;
  document.getElementById('quadSouthLabel').textContent = translations[lang].quadSouth;
  document.getElementById('quadWestLabel').textContent = translations[lang].quadWest;
  document.getElementById('quadDiagLabel').textContent = translations[lang].quadDiag;
  document.getElementById('irregShareLabel').textContent = translations[lang].irregShare;
  document.getElementById('triT1Label').textContent = translations[lang].triT1;
  document.getElementById('triT2Label').textContent = translations[lang].triT2;
  document.getElementById('triT3Label').textContent = translations[lang].triT3;
  document.getElementById('triShareLabel').textContent = translations[lang].triShare;
  document.getElementById('pentaABLabel').textContent = translations[lang].pentaAB;
  document.getElementById('pentaBCLabel').textContent = translations[lang].pentaBC;
  document.getElementById('pentaCDLabel').textContent = translations[lang].pentaCD;
  document.getElementById('pentaDELabel').textContent = translations[lang].pentaDE;
  document.getElementById('pentaEALabel').textContent = translations[lang].pentaEA;
  document.getElementById('pentaD1Label').textContent = translations[lang].pentaD1;
  document.getElementById('pentaD2Label').textContent = translations[lang].pentaD2;
  document.getElementById('pentShareLabel').textContent = translations[lang].pentShare;
  document.getElementById('pondDistanceLabel').textContent = translations[lang].pondDistance;
  document.getElementById('pondShareLabel').textContent = translations[lang].pondShare;
  document.getElementById('polyShareLabel').textContent = translations[lang].polyShare;
  const sidesContainer = document.getElementById('polySidesContainer');
  const currentSides = Array.from(sidesContainer.querySelectorAll('.poly-side-input')).map(e => e.value);
  sidesContainer.innerHTML = '';
  for (let i = 1; i <= polySideIndex; i++) {
    const div = document.createElement('div');
    div.className = 'poly-side';
    const sideLabel = String.fromCharCode(65 + i - 1) + String.fromCharCode(65 + i);
    div.innerHTML = `<label>${translations[lang].polySide.replace('{side}', sideLabel)}</label><input type="number" min="1" class="poly-side-input" value="${currentSides[i-1] || ''}" />`;
    sidesContainer.appendChild(div);
  }
  updatePolyDiags();
  updateHistoryList();
}

/* --- Initialize --- */
selectShape('rect');
toggleLanguage();
window.addEventListener('resize', () => {
  resizeCanvas();
  const shape = Object.keys(sections).find(key => document.getElementById(sections[key]).classList.contains('visible'));
  if (domCache.result.style.display === 'block') {
    const inputs = {};
    document.querySelectorAll('input').forEach(input => {
      if (input.value) inputs[input.id] = input.value;
    });
    if (shape === 'poly') {
      inputs.sides = Array.from(document.querySelectorAll('.poly-side-input')).map(e => e.value);
      inputs.diags = Array.from(document.querySelectorAll('.poly-diag-input')).map(e => e.value);
    }
    if (shape === 'pond') {
      inputs.heights = Array.from(document.querySelectorAll('.pond-height')).map(e => e.value);
    }
    if (shape === 'quad') {
      inputs.method = quadMethod;
    }
    drawResultFigure(shape, inputs, domCache.result.innerText.split('\n')[2]?.replace('â€¢ ', '') || '');
  }
});
resizeCanvas();
if (JSON.parse(localStorage.getItem('history') || '[]').length === 0) {
  domCache.historyBtn.disabled = true;
}
</script>
</body>
</html>
