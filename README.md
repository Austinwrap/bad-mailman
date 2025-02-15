<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>BAD MAILMAN - Ultimate Chaos</title>
  <style>
    /* Overall Theme: Pastel base with chaotic accents */
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: #fce4ec;
      margin: 0;
      padding: 0;
      text-align: center;
      transition: background 0.5s;
    }
    #game-container {
      max-width: 600px;
      margin: 20px auto;
      padding: 15px;
      background: #ffffff;
      border-radius: 10px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
      transition: background-color 0.5s;
    }
    h1 {
      color: #d81b60;
      margin-bottom: 10px;
    }
    /* Canvas Game Styles */
    #gameCanvas {
      background: #e1bee7;
      border: 2px solid #d81b60;
      border-radius: 5px;
      margin-bottom: 10px;
    }
    /* Stats Panel (shared for both parts) */
    #stats {
      background: #f8bbd0;
      border: 1px solid #f06292;
      padding: 10px;
      margin-bottom: 10px;
      text-align: left;
      border-radius: 5px;
    }
    #stats p {
      font-size: 1.2em;
      margin: 5px 0;
    }
    /* Display for Aisle/Objective */
    #aisleDisplay, #objectiveDisplay {
      font-weight: bold;
      margin-bottom: 10px;
      color: #6a1b9a;
      font-size: 1.1em;
    }
    /* Additional display for Mailbox Collisions */
    #collisionDisplay {
      font-weight: bold;
      margin-bottom: 10px;
      color: #e53935;
      font-size: 1.1em;
    }
    /* Action Buttons (Clicking Game) */
    #actions {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 10px;
      margin-bottom: 20px;
    }
    #actions button {
      flex: 1 1 calc(45% - 10px);
      padding: 10px;
      font-size: 1em;
      border: none;
      border-radius: 10px;
      background: linear-gradient(45deg, #ff6f91, #ff9671);
      color: #fff;
      cursor: pointer;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      transition: transform 0.2s, box-shadow 0.2s;
    }
    #actions button:hover {
      box-shadow: 0 6px 8px rgba(0, 0, 0, 0.15);
    }
    #actions button:active {
      transform: scale(0.95);
    }
    #actions button:disabled {
      background: #ccc;
      cursor: not-allowed;
    }
    /* Activity Log */
    #log {
      background: #fff;
      border: 1px solid #f06292;
      padding: 10px;
      max-height: 200px;
      overflow-y: auto;
      text-align: left;
      border-radius: 5px;
    }
    #log h2 {
      margin-top: 0;
      font-size: 1.2em;
      border-bottom: 1px solid #f06292;
      padding-bottom: 5px;
    }
    #log ul {
      list-style-type: none;
      padding: 0;
      margin: 0;
      font-size: 0.9em;
    }
    #log li {
      margin: 3px 0;
    }
    /* Gamble Modal Styles */
    #gambleModal {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0,0,0,0.7);
      display: none;
      justify-content: center;
      align-items: center;
      z-index: 100;
    }
    #gambleModalContent {
      background: #fff;
      padding: 20px;
      border-radius: 10px;
      text-align: center;
      max-width: 300px;
      width: 80%;
    }
    #gambleModalContent p {
      font-size: 1.2em;
      margin-bottom: 15px;
    }
    #gambleModalContent button {
      margin: 10px;
      padding: 10px 20px;
      font-size: 1em;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    #gambleYes {
      background: #43a047;
      color: #fff;
    }
    #gambleNo {
      background: #e53935;
      color: #fff;
    }
  </style>
</head>
<body>
  <div id="game-container">
    <h1>BAD MAILMAN - Ultimate Chaos</h1>
    
    <!-- Canvas Game: Mail Truck & Mailboxes -->
    <canvas id="gameCanvas"></canvas>
    
    <!-- Shared Stats Panel -->
    <div id="stats">
      <p>Health: <span id="health">100</span></p>
      <p>Cash: $<span id="cash">50</span></p>
      <p>Level: <span id="level">1</span></p>
      <p>Time Left: <span id="timer">300</span> s</p>
    </div>
    
    <!-- Display for Aisle & Objective -->
    <div id="aisleDisplay">Aisle: None</div>
    <div id="objectiveDisplay">Goal: Deliver mail, earn cash, and level up!</div>
    <!-- Display for Mailbox Collisions (the truck must avoid mailboxes) -->
    <div id="collisionDisplay">Mailbox Collisions: <span id="collisions">0</span> / 3</div>
    
    <!-- Action Buttons (Clicking Game) -->
    <div id="actions">
      <button id="deliverMail">Deliver Mail</button>
      <button id="drinkWater">Drink Water</button>
      <button id="takeLunch">Take Lunch Break</button>
      <button id="arriveDrop">Arrive at Mail Drop</button>
      <button id="returnOffice">Return to Office</button>
      <button id="driveCarefully">Drive Carefully</button>
      <button id="petCat">Pet Cat</button>
      <button id="playDog">Play with Dog</button>
      <button id="takeDrink">Take a Drink</button>
      <button id="skipDelivery">Skip Delivery</button>
      <button id="returnLate">Return Late</button>
      <button id="driveRecklessly">Drive Recklessly</button>
    </div>
    
    <!-- Activity Log -->
    <div id="log">
      <h2>Activity Log</h2>
      <ul id="logList"></ul>
    </div>
  </div>
  
  <!-- Gamble Modal -->
  <div id="gambleModal">
    <div id="gambleModalContent">
      <p>Gamble for a better reward?</p>
      <button id="gambleYes">Yes</button>
      <button id="gambleNo">No</button>
    </div>
  </div>
  
  <script>
    /* ===============================
       GLOBAL GAME STATE VARIABLES
    ================================ */
    let health = 100,
        cash = 50,
        experience = 0,
        level = 1,
        timer = 300,    // seconds
        clickCount = 0,
        mailboxCollisions = 0,  // count collisions in canvas game
        collisionThreshold = 3; // if collisions reach this, game over
    
    /* ===============================
       CANVAS GAME SETUP (Mail Truck & Mailboxes)
    ================================ */
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");
    // Set canvas size relative to container width (adjust as needed)
    canvas.width = window.innerWidth * 0.9;
    canvas.height = 300;
    
    // Mail Truck object (controlled by arrow keys; no gravity here)
    let mailTruck = {
      x: 50,
      y: canvas.height / 2 - 25,
      width: 50,
      height: 50,
      speed: 5,
      color: "#d81b60"
    };
    
    // Array for mailboxes (obstacles)
    let mailboxes = [];
    let mailboxTimer = 0;
    
    function spawnMailbox() {
      let mailbox = {
        x: canvas.width,
        // Random y position within the canvas (leaving a little margin)
        y: getRandomInt(10, canvas.height - 50 - 10),
        width: 40,
        height: 40,
        speed: 4 + Math.random() * 3
      };
      mailboxes.push(mailbox);
    }
    
    function updateCanvasGame() {
      // Spawn mailboxes periodically
      mailboxTimer++;
      if (mailboxTimer > 80) { // adjust frequency as desired
        spawnMailbox();
        mailboxTimer = 0;
      }
      
      // Update mailboxes: move left
      for (let i = mailboxes.length - 1; i >= 0; i--) {
        mailboxes[i].x -= mailboxes[i].speed;
        // Remove if off-screen
        if (mailboxes[i].x + mailboxes[i].width < 0) {
          mailboxes.splice(i, 1);
        }
        // Collision detection with mailTruck
        if (
          mailTruck.x < mailboxes[i].x + mailboxes[i].width &&
          mailTruck.x + mailTruck.width > mailboxes[i].x &&
          mailTruck.y < mailboxes[i].y + mailboxes[i].height &&
          mailTruck.y + mailTruck.height > mailboxes[i].y
        ) {
          mailboxCollisions++;
          updateCollisionDisplay();
          logAction("Mailbox collision! (" + mailboxCollisions + "/" + collisionThreshold + ")");
          mailboxes.splice(i, 1);
          if (mailboxCollisions >= collisionThreshold) {
            logAction("GAME OVER: Too many mailbox collisions!");
            gameOver(); // end entire game
          }
        }
      }
    }
    
    function drawCanvasGame() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      // Draw ground line
      ctx.fillStyle = "#654321";
      ctx.fillRect(0, canvas.height - 10, canvas.width, 10);
      // Draw mail truck
      ctx.fillStyle = mailTruck.color;
      ctx.fillRect(mailTruck.x, mailTruck.y, mailTruck.width, mailTruck.height);
      // Draw mailboxes
      ctx.fillStyle = "#000";
      mailboxes.forEach(box => {
        ctx.fillRect(box.x, box.y, box.width, box.height);
      });
    }
    
    function canvasGameLoop() {
      updateCanvasGame();
      drawCanvasGame();
      requestAnimationFrame(canvasGameLoop);
    }
    canvasGameLoop();
    
    // ------------------------------
    // Mail Truck Control (Arrow keys)
    // ------------------------------
    document.addEventListener("keydown", function(e) {
      // Up Arrow
      if (e.key === "ArrowUp") {
        mailTruck.y -= mailTruck.speed;
        if (mailTruck.y < 0) mailTruck.y = 0;
      }
      // Down Arrow
      if (e.key === "ArrowDown") {
        mailTruck.y += mailTruck.speed;
        if (mailTruck.y + mailTruck.height > canvas.height - 10) {
          mailTruck.y = canvas.height - 10 - mailTruck.height;
        }
      }
    });
    
    /* ===============================
       UTILITY FUNCTIONS
    ================================ */
    function getRandomInt(min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    }
    
    function updateStats() {
      document.getElementById("health").textContent = health;
      document.getElementById("cash").textContent = cash;
      document.getElementById("level").textContent = level;
      document.getElementById("timer").textContent = timer;
    }
    
    function updateAisleDisplay() {
      const aisles = ["North Side", "East End", "West Side", "Downtown", "Uptown", "Suburban", "Industrial", "Central Park", "Lakeside", "Highway"];
      const randomAisle = aisles[getRandomInt(0, aisles.length - 1)];
      document.getElementById("aisleDisplay").textContent = "Aisle: " + randomAisle;
    }
    
    function updateCollisionDisplay() {
      document.getElementById("collisions").textContent = mailboxCollisions;
    }
    
    function logAction(text) {
      const li = document.createElement("li");
      li.textContent = text;
      document.getElementById("logList").appendChild(li);
      const logDiv = document.getElementById("log");
      logDiv.scrollTop = logDiv.scrollHeight;
    }
    
    /* ===============================
       GAME OVER FUNCTION
    ================================ */
    function gameOver() {
      logAction("GAME OVER: You got fired and died!");
      disableButtons();
    }
    
    function disableButtons() {
      const buttons = document.querySelectorAll("#actions button");
      buttons.forEach(btn => btn.disabled = true);
    }
    
    function updateLevel() {
      level = Math.floor(experience / 10) + 1;
    }
    
    function shuffleButtons() {
      const container = document.getElementById("actions");
      const buttons = Array.from(container.children);
      for (let i = buttons.length - 1; i > 0; i--) {
        const j = getRandomInt(0, i);
        container.insertBefore(buttons[j], buttons[i]);
      }
    }
    
    function applyChaos() {
      // Random transforms for action buttons
      const buttons = document.querySelectorAll("#actions button");
      buttons.forEach(btn => {
        const randomRotation = getRandomInt(-45, 45);
        const randomX = getRandomInt(-20, 20);
        const randomY = getRandomInt(-20, 20);
        btn.style.transform = `rotate(${randomRotation}deg) translate(${randomX}px, ${randomY}px)`;
      });
      // Randomize background colors of container and page
      const container = document.getElementById("game-container");
      const randomHue = getRandomInt(0, 360);
      container.style.backgroundColor = `hsl(${randomHue}, 70%, 95%)`;
      document.body.style.background = `hsl(${getRandomInt(0,360)}, 60%, 90%)`;
    }
    
    /* ===============================
       GAMBLE MODAL FUNCTIONS
    ================================ */
    function showGambleModal() {
      document.getElementById("gambleModal").style.display = "flex";
    }
    function hideGambleModal() {
      document.getElementById("gambleModal").style.display = "none";
    }
    function gambleBonus() {
      const bonusCash = getRandomInt(30, 50);
      const bonusHealth = getRandomInt(5, 10);
      const bonusExp = getRandomInt(2, 5);
      cash += bonusCash;
      health = Math.min(health + bonusHealth, 100);
      experience += bonusExp;
      updateLevel();
      updateStats();
      logAction("GAMBLE BONUS: You won! (+" + bonusCash + " Cash, +" + bonusHealth + " Health, +" + bonusExp + " EXP)");
    }
    
    /* ===============================
       ACTION FUNCTIONS (Clicking Game)
    ================================ */
    function performGoodAction(actionName) {
      const goodActions = [
        "Deliver mail on time – Earn bonus points!",
        "Follow traffic laws – Stay safe on the road.",
        "Pet a friendly cat – Boost your morale.",
        "Play with a happy dog – Lift your energy.",
        "Drink water – Keep your health up.",
        "Take a proper lunch break – Recharge your energy.",
        "Arrive at a new mail drop on time – Unlock bonuses.",
        "Return to the office promptly – Impress your boss.",
        "Drive the mail truck carefully – Avoid mishaps.",
        "Deliver packages accurately – Earn extra cash."
      ];
      const message = goodActions[getRandomInt(0, goodActions.length - 1)];
      const healthGain = getRandomInt(3, 7);
      const cashGain = getRandomInt(10, 20);
      health = Math.min(health + healthGain, 100);
      cash += cashGain;
      experience++;
      updateLevel();
      updateStats();
      logAction("GOOD: " + actionName + " — " + message + " (+" + healthGain + " Health, +$" + cashGain + ")");
      // Simulate truck "jump" by nudging its position (for fun)
      mailTruck.y -= 10;
      if (mailTruck.y < 0) mailTruck.y = 0;
    }
    
    function performBadAction(actionName) {
      const badActions = [
        "Drink on the job – Immediately lowers your health.",
        "Skip a scheduled delivery – Lose reputation.",
        "Deliver to the wrong address – Lose cash.",
        "Be late to a mail drop – Incur time penalties.",
        "Run over a pedestrian – Massive penalties!",
        "Ignore traffic signals – Risk an accident."
      ];
      const message = badActions[getRandomInt(0, badActions.length - 1)];
      let healthLoss = getRandomInt(10, 20);
      let cashLoss = getRandomInt(5, 15);
      if (message.toLowerCase().includes("nap") || message.toLowerCase().includes("run over") ||
          message.toLowerCase().includes("drink and drive") || message.toLowerCase().includes("instant")) {
        healthLoss = health;
      }
      health = Math.max(health - healthLoss, 0);
      cash = Math.max(cash - cashLoss, 0);
      if (experience > 0) experience--;
      updateLevel();
      updateStats();
      logAction("BAD: " + actionName + " — " + message + " (-" + healthLoss + " Health, -$" + cashLoss + ")");
    }
    
    function performRecklessAction() {
      const healthLoss = getRandomInt(15, 25);
      const cashLoss = getRandomInt(10, 20);
      health = Math.max(health - healthLoss, 0);
      cash = Math.max(cash - cashLoss, 0);
      if (experience > 0) experience--;
      updateLevel();
      updateStats();
      logAction("BAD: Drive Recklessly — You lost control! (-" + healthLoss + " Health, -$" + cashLoss + ")");
    }
    
    /* ===============================
       TIMER (Clicking Game)
    ================================ */
    setInterval(function(){
      if (timer > 0) {
        timer--;
        updateStats();
        if (timer <= 0) {
          checkGameOver();
        }
      }
    }, 1000);
    
    /* ===============================
       POST-ACTION UPDATE (Clicking Game)
       Increment click count, update aisle, shuffle buttons, log a tip, and apply chaos.
    ================================ */
    function postActionUpdate() {
      clickCount++;
      updateAisleDisplay();
      shuffleButtons();
      const tipSuggestions = [
        "Tip: Try 'Deliver Mail' for a cash boost!",
        "Tip: 'Drink Water' can keep your health high.",
        "Tip: 'Take Lunch Break' recharges you.",
        "Tip: 'Drive Carefully' earns extra experience.",
        "Tip: Avoid 'Skip Delivery' for steady progress."
      ];
      const tip = tipSuggestions[getRandomInt(0, tipSuggestions.length - 1)];
      logAction(tip);
      if (clickCount % 4 === 0) {
        showGambleModal();
      }
      applyChaos();
    }
    
    /* ===============================
       BUTTON EVENT LISTENERS (Clicking Game)
    ================================ */
    document.getElementById("deliverMail").addEventListener("click", function() {
      performGoodAction("Deliver Mail");
      postActionUpdate();
    });
    document.getElementById("drinkWater").addEventListener("click", function() {
      performGoodAction("Drink Water");
      postActionUpdate();
    });
    document.getElementById("takeLunch").addEventListener("click", function() {
      performGoodAction("Take Lunch Break");
      postActionUpdate();
    });
    document.getElementById("arriveDrop").addEventListener("click", function() {
      performGoodAction("Arrive at Mail Drop");
      postActionUpdate();
    });
    document.getElementById("returnOffice").addEventListener("click", function() {
      performGoodAction("Return to Office");
      postActionUpdate();
    });
    document.getElementById("driveCarefully").addEventListener("click", function() {
      performGoodAction("Drive Carefully");
      postActionUpdate();
    });
    document.getElementById("petCat").addEventListener("click", function() {
      performGoodAction("Pet Cat");
      postActionUpdate();
    });
    document.getElementById("playDog").addEventListener("click", function() {
      performGoodAction("Play with Dog");
      postActionUpdate();
    });
    document.getElementById("takeDrink").addEventListener("click", function() {
      performBadAction("Take a Drink");
      postActionUpdate();
    });
    document.getElementById("skipDelivery").addEventListener("click", function() {
      performBadAction("Skip Delivery");
      postActionUpdate();
    });
    document.getElementById("returnLate").addEventListener("click", function() {
      performBadAction("Return Late");
      postActionUpdate();
    });
    document.getElementById("driveRecklessly").addEventListener("click", function() {
      performRecklessAction();
      postActionUpdate();
    });
    
    /* ===============================
       GAMBLE MODAL EVENT LISTENERS
    ================================ */
    document.getElementById("gambleYes").addEventListener("click", function() {
      gambleBonus();
      hideGambleModal();
    });
    document.getElementById("gambleNo").addEventListener("click", function() {
      logAction("GAMBLE: You declined the gamble.");
      hideGambleModal();
    });
    
    // Initial Setup
    updateStats();
    updateAisleDisplay();
    updateCollisionDisplay();
  </script>
</body>
</html>
