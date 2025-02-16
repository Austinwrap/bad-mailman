<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>BAD MAILMAN - Neon Decisions 🇺🇸</title>
  <style>
    /* American Flag Red, White & Blue Theme */
    body {
      margin: 0;
      padding: 0;
      background: #001f3f; /* Navy Blue */
      color: #fff;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      text-align: center;
    }
    #game-container {
      max-width: 600px;
      margin: 20px auto;
      padding: 15px;
      background: #fff; /* White container */
      border: 3px solid #b22234; /* Bold Red */
      border-radius: 10px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.7);
    }
    h1 {
      margin-bottom: 10px;
      color: #3c3b6e; /* Dark Blue */
    }
    /* Stats Panel */
    #stats {
      background: #f7f7f7;
      border: 2px solid #b22234;
      padding: 10px;
      margin-bottom: 10px;
      text-align: left;
      border-radius: 5px;
      color: #001f3f;
    }
    #stats p {
      font-size: 1.2em;
      margin: 5px 0;
    }
    /* Progress Display */
    #progress {
      font-size: 1em;
      font-weight: bold;
    }
    /* Objective Display */
    #objectiveDisplay {
      font-weight: bold;
      margin-bottom: 10px;
      color: #b22234;
      font-size: 1.1em;
    }
    /* Instructions */
    #instructions {
      font-size: 1em;
      margin-bottom: 10px;
      color: #001f3f;
    }
    /* Upcoming Tasks Display */
    #upcomingTasks {
      background: #f7f7f7;
      border: 2px dashed #b22234;
      padding: 10px;
      margin-bottom: 10px;
      border-radius: 5px;
      color: #001f3f;
      font-size: 1em;
    }
    /* Action Buttons Container */
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
      border: 2px solid #b22234;
      border-radius: 10px;
      background: #b22234;
      color: #fff;
      cursor: pointer;
      transition: transform 0.2s, box-shadow 0.2s;
    }
    #actions button:hover {
      box-shadow: 0 0 12px #3c3b6e;
    }
    #actions button:active {
      transform: scale(0.95);
    }
    #actions button:disabled {
      background: #888;
      border-color: #666;
      cursor: not-allowed;
    }
    /* Activity Log */
    #log {
      background: #f7f7f7;
      border: 2px solid #b22234;
      padding: 10px;
      max-height: 200px;
      overflow-y: auto;
      text-align: left;
      border-radius: 5px;
      color: #001f3f;
    }
    #log h2 {
      margin-top: 0;
      font-size: 1.2em;
      border-bottom: 2px solid #b22234;
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
    /* Neon Pop-up Modal */
    #popupModal {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0,0,0,0.95);
      display: none;
      justify-content: center;
      align-items: center;
      z-index: 100;
    }
    /* Chaotic, Flashy Popup (American Flag Theme) */
    #popupModalContent {
      background: linear-gradient(45deg, #fff, #b22234, #fff, #3c3b6e);
      color: #001f3f;
      padding: 20px;
      border: 3px solid #3c3b6e;
      border-radius: 10px;
      text-align: center;
      max-width: 400px;
      animation: chaoticPulse 0.5s infinite alternate;
    }
    #popupModalContent p {
      font-size: 1.1em;
      margin-bottom: 20px;
    }
    .popup-option {
      margin: 10px;
      padding: 10px 20px;
      font-size: 1em;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      background: #b22234;
      color: #fff;
      transition: background 0.3s;
    }
    .popup-option:hover {
      background: #3c3b6e;
    }
    /* Decision Timer */
    #decisionTimer {
      font-size: 1em;
      margin-top: 10px;
      color: #b22234;
    }
    @keyframes chaoticPulse {
      0% { transform: scale(1) rotate(0deg); box-shadow: 0 0 10px #3c3b6e; }
      25% { transform: scale(1.1) rotate(3deg); box-shadow: 0 0 20px #ff0000; }
      50% { transform: scale(1) rotate(-3deg); box-shadow: 0 0 30px #ffffff; }
      75% { transform: scale(1.1) rotate(3deg); box-shadow: 0 0 20px #3c3b6e; }
      100% { transform: scale(1) rotate(0deg); box-shadow: 0 0 10px #b22234; }
    }
  </style>
</head>
<body>
  <div id="game-container">
    <h1>BAD MAILMAN 🇺🇸</h1>
    
    <!-- Stats Panel -->
    <div id="stats">
      <p>Health: <span id="health">100</span></p>
      <p>Cash: $<span id="cash">50</span></p>
      <p>Level: <span id="level">1</span></p>
      <p>Time Left: <span id="timer">120</span> s</p>
      <p id="progress"></p>
    </div>
    
    <!-- Objective Display -->
    <div id="objectiveDisplay">
      Complete 5 deliveries and 5 healthy tasks to level up!<br>
      Next Delivery: Deliver to <span id="currentAddress"></span> 🇺🇸
    </div>
    
    <!-- Upcoming Tasks Display (for deliveries) -->
    <div id="upcomingTasks"></div>
    
    <!-- Instructions -->
    <p id="instructions">Objective: Complete 5 delivery tasks AND 5 healthy tasks (e.g., Drink Water, Take Lunch Break, Play with Dog) each level.</p>
    
    <!-- Action Buttons Panel -->
    <div id="actions">
      <button id="deliverMail">Deliver Mail</button>
      <button id="drinkWater">Drink Water</button>
      <button id="takeLunch">Take Lunch Break</button>
      <button id="skipDelivery">Skip Delivery</button>
      <button id="driveCarefully">Drive Carefully</button>
      <button id="returnOffice">Return to Office</button>
      <button id="playWithDog">Play with Dog</button>
      <button id="takeADrink">Take a Drink</button>
    </div>
    
    <!-- Activity Log -->
    <div id="log">
      <h2>Activity Log 🇺🇸</h2>
      <ul id="logList"></ul>
    </div>
  </div>
  
  <!-- Neon Pop-up Modal -->
  <div id="popupModal">
    <div id="popupModalContent">
      <p id="popupText"></p>
      <div id="popupOptions"></div>
      <div id="decisionTimer">Time: <span id="decisionTime">5</span>s</div>
    </div>
  </div>
  
  <script>
    /*******************
     * GLOBAL GAME STATE
     *******************/
    let health = 100,
        cash = 50,
        experience = 0,
        level = 1,
        timer = 120, // Overall game timer (seconds)
        clickCount = 0;
    let decisionTime = 5; // Popup decision timer (seconds)
    let decisionInterval = null;
    
    // Level progress variables:
    const requiredDeliveryTasks = 5;
    const requiredHealthyTasks = 5;
    let deliveryCount = 0;
    let healthyCount = 0;
    
    // To track the active custom scenario
    let currentScenarioKey = "";
    
    // Timer for the current delivery task (upcoming tasks)
    let taskTimer = 30; // seconds
    
    // UTILITY FUNCTION
    function getRandomInt(min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    }
    
    // ADDRESS LIST (50 Bristol, CT Streets)
    const addresses = [
      "1 Main St", "2 Broad St", "3 Church St", "4 Washington St", "5 West St",
      "6 East St", "7 North St", "8 South St", "9 Maple Ave", "10 Oak St",
      "11 Pine St", "12 Cedar St", "13 Birch St", "14 Elm St", "15 Spruce St",
      "16 Cherry St", "17 Walnut St", "18 Chestnut St", "19 Hickory St", "20 Sycamore St",
      "21 Main St", "22 Broadway", "23 King St", "24 Queen St", "25 Irving St",
      "26 Jerome Ave", "27 Illinois Ave", "28 Maple Dr", "29 Park Ave", "30 Sunset Blvd",
      "31 Main Ave", "32 Oak Ave", "33 Elm Ave", "34 Church Ave", "35 Washington Ave",
      "36 West Ave", "37 East Ave", "38 North Ave", "39 South Ave", "40 Central St",
      "41 Union St", "42 Prospect St", "43 Pleasant St", "44 River Rd", "45 Mill St",
      "46 Bridge St", "47 Mill Rd", "48 Mill Ave", "49 College St", "50 Market St"
    ];
    let currentAddress = addresses[getRandomInt(0, addresses.length - 1)];
    
    // Helper to get a random address excluding a given one
    function getRandomAddressExcluding(excluded) {
      let filtered = addresses.filter(addr => addr !== excluded);
      return filtered[getRandomInt(0, filtered.length - 1)];
    }
    
    // GOOD MAILMAN ACTIVITIES (50 items)
    const goodActivities = [
      "Deliver mail on time – Earn bonus points!",
      "Follow all traffic laws – Stay safe and avoid penalties.",
      "Pet a friendly cat – Boosts your morale and health.",
      "Play with a happy dog – Increases your energy.",
      "Drink water – Keep hydrated and maintain health.",
      "Take a proper lunch break – Restores energy.",
      "Arrive at a new mail drop on time – Unlocks route bonuses.",
      "Return to the office promptly – Gains supervisor approval.",
      "Drive the mail truck carefully – Prevents accidents.",
      "Deliver packages accurately – Earns extra cash.",
      "Greet customers cheerfully – Boosts customer tips!",
      "Sort mail efficiently – Speeds up your route.",
      "Take rejuvenating short breaks – Recovers health.",
      "Refuel the truck promptly – Avoids breakdowns.",
      "Pay attention to road signs – Keeps you on track.",
      "Accept friendly tips from customers – Increases cash balance.",
      "Avoid distractions – Maintains focus on the task.",
      "Listen to motivational podcasts – Boosts your overall performance.",
      "Follow GPS directions accurately – Saves time.",
      "Park the truck safely – Avoids minor accidents.",
      "Take regular health checks – Ensures you’re in top shape.",
      "Keep the vehicle clean – Impresses your supervisor.",
      "Enjoy scenic routes – Provides a temporary health boost.",
      "Handle fragile packages with care – Gains extra rewards.",
      "Plan your route efficiently – Saves time and energy.",
      "Help elderly neighbors with their mail – Earns community points.",
      "Pick up additional packages correctly – Boosts cash earnings.",
      "Smile at every doorstep – Increases your reputation.",
      "Verify addresses twice – Prevents delivery errors.",
      "Offer assistance to confused customers – Gains extra tips.",
      "Receive compliments on your service – Improves job stability.",
      "Follow supervisor instructions – Maintains employment.",
      "Avoid speeding – Keeps you safe and on schedule.",
      "Maintain positive energy – Improves overall performance.",
      "Keep a tidy uniform – Impresses your boss.",
      "Interact nicely with coworkers – Builds team support.",
      "Enjoy a balanced, healthy snack – Boosts your stamina.",
      "Appreciate nice weather – Enhances your mood.",
      "Listen to uplifting music – Increases focus.",
      "Follow proper safety protocols – Reduces risk of accidents.",
      "Double-check deliveries – Prevents mistakes.",
      "Navigate difficult streets carefully – Maintains health.",
      "Take time to pet friendly dogs – Provides an energy boost.",
      "Use your map effectively – Avoids getting lost.",
      "Offer extra help during inclement weather – Gains bonus rewards.",
      "Stay alert to avoid accidents – Keeps you in the game.",
      "Practice defensive driving – Improves safety stats.",
      "Participate in friendly competitions – Earns extra cash.",
      "Earn daily bonuses for perfect shifts – Increases your level.",
      "Maintain a healthy cash balance – Allows for upgrades and bonuses."
    ];
    
    // BAD MAILMAN ACTIVITIES (50 items)
    const badActivities = [
      "Drink on the job – Immediately lowers your health.",
      "Skip a scheduled mail delivery – Risks customer complaints.",
      "Deliver to the wrong address – Costs cash and reputation.",
      "Be late to a mail drop – Causes time penalties.",
      "Run over a pedestrian – Massive health and job penalties.",
      "Ignore traffic signals – Leads to accidents.",
      "Text while driving – Distracts you and reduces health.",
      "Overindulge in shots – Drastically depletes your energy.",
      "Accept under-the-table drinks – Invites trouble with the boss.",
      "Get lost on your route – Wastes precious time.",
      "Arrive too early or too late at the office – Displeases your supervisor.",
      "Ignore a cat’s need for petting – Lowers your morale.",
      "Deliver mail to the wrong mailbox – Causes rework and penalties.",
      "Get into a fight with a customer – Reduces both health and cash.",
      "Miss a scheduled water break – Causes dehydration penalties.",
      "Skip your lunch break – Drains your energy.",
      "Get pulled over for a traffic violation – Costs cash and time.",
      "Take a dangerous detour – Increases risk of accidents.",
      "Forget to refuel the truck – Risks a breakdown.",
      "Stop at a bar instead of the office – Hurts your job standing.",
      "Speed excessively – Increases accident risk.",
      "Collide with obstacles (like trees) – Drastically lowers your health.",
      "Fail to signal turns – Risks causing crashes.",
      "Hit potholes recklessly – Damages your vehicle and health.",
      "Skip essential vehicle safety checks – Increases accident potential.",
      "Answer personal calls while driving – Distracts you and wastes time.",
      "Tear up the mail bag – Costs extra repair time.",
      "Take unauthorized long breaks – Frustrates your supervisor.",
      "Forget to deliver registered mail – Results in penalties.",
      "Mess up the mail order – Confuses customers and loses cash.",
      "Drop mail on the ground – Lowers customer satisfaction.",
      "Engage in street racing – Drains your cash with fines.",
      "Disobey traffic laws blatantly – Increases accident risk.",
      "Ignore supervisor instructions – Endangers your job.",
      "Take risky shortcuts – Leads to unexpected hazards.",
      "Overgive change to customers – Loses cash unnecessarily.",
      "Lose your mail keys – Causes delays and penalties.",
      "Misplace a crucial package – Reduces customer trust.",
      "Argue with coworkers – Affects team morale.",
      "Ram a parked vehicle – Causes heavy repairs and cash loss.",
      "Take unapproved detours – Confuses the route.",
      "Ignore customer instructions – Results in delivery errors.",
      "Overeat junk food on the job – Drains your health.",
      "Fail to pick up an urgent package – Costs extra time.",
      "Drink and drive – Drastically reduces your health.",
      "Take a nap on duty – Gets you fired instantly.",
      "Overspend on personal expenses during work – Drains your cash balance.",
      "Engage in illegal side hustles – Risks job termination.",
      "Drive with reckless abandon – Increases accident frequency.",
      "Neglect scheduled work responsibilities – Leads to instant game over."
    ];
    
    // EXTRA OPTIONS (e.g., Gamble, Smoke Weed, Do Cocaine)
    const extraOptions = [
      { text: "Gamble", effect: { health: -5, cash: +30, exp: +2 }, outcome: "You gambled and got lucky!" },
      { text: "Smoke Weed", effect: { health: -10, cash: 0, exp: +1 }, outcome: "You smoked weed—your focus wavered." },
      { text: "Do Cocaine", effect: { health: -15, cash: +20, exp: +1 }, outcome: "You did cocaine; cash flowed but your health suffered." }
    ];
    
    // POP-UP SCENARIOS
    const popups = [
      {
        type: "delivery",
        text: "Next: Deliver to [ADDRESS]. Choose the correct address:",
        options: [] // We'll generate delivery options dynamically.
      },
      {
        type: "activity",
        text: "Choose your next action:",
        options: []  // To be filled dynamically.
      },
      {
        type: "goBad",
        text: "You're feeling rebellious! What do you do?",
        options: [
          { text: "Gamble", effect: { health: -5, cash: +30, exp: +2 }, outcome: "You gamble and enjoy the thrill!", delivery: false },
          { text: "Drink alcohol", effect: { health: -10, cash: +20, exp: +1 }, outcome: "You drink on the job—chaos ensues!", delivery: false },
          { text: "Do cocaine", effect: { health: -15, cash: +20, exp: +1 }, outcome: "You did cocaine; it's a wild ride!", delivery: false },
          { text: "Stay on track", effect: { health: +5, cash: 0, exp: +2 }, outcome: "You decide to keep your focus.", delivery: false }
        ]
      }
    ];
    
    // NEW FUNCTION: showDeliveryPopup
    function showDeliveryPopup(scenario) {
      // Replace token with current address.
      let text = scenario.text.replace("[ADDRESS]", currentAddress);
      document.getElementById("popupText").textContent = text;
      
      // Create exactly three options: one correct, two wrong.
      let correctOption = { 
        text: currentAddress, 
        effect: { health: 10, cash: 20, exp: 5 }, 
        outcome: "You deliver the mail perfectly!", 
        delivery: true 
      };
      let wrongOption1 = { 
        text: getRandomAddressExcluding(currentAddress), 
        effect: { health: -10, cash: -5, exp: 1 }, 
        outcome: "Wrong address! Delivery failed.", 
        delivery: false 
      };
      let wrongOption2 = { 
        text: getRandomAddressExcluding(currentAddress), 
        effect: { health: -10, cash: -5, exp: 1 }, 
        outcome: "Wrong address! Delivery failed.", 
        delivery: false 
      };
      
      let optionsArray = [correctOption, wrongOption1, wrongOption2];
      
      // Shuffle the options.
      for (let i = optionsArray.length - 1; i > 0; i--) {
        const j = getRandomInt(0, i);
        [optionsArray[i], optionsArray[j]] = [optionsArray[j], optionsArray[i]];
      }
      
      let optionsDiv = document.getElementById("popupOptions");
      optionsDiv.innerHTML = "";
      optionsArray.forEach(option => {
        let btn = document.createElement("button");
        btn.textContent = option.text;
        btn.className = "popup-option";
        btn.addEventListener("click", function() {
          clearInterval(decisionInterval);
          processPopupChoice(option);
        });
        optionsDiv.appendChild(btn);
      });
      
      decisionTime = 5;
      document.getElementById("decisionTime").textContent = decisionTime;
      document.getElementById("popupModal").style.display = "flex";
      
      decisionInterval = setInterval(function() {
        decisionTime--;
        document.getElementById("decisionTime").textContent = decisionTime;
        if (decisionTime <= 0) {
          clearInterval(decisionInterval);
          let defaultEffect = { health: -15, cash: -10, exp: 0 };
          health += defaultEffect.health;
          cash += defaultEffect.cash;
          logAction("You hesitated! Lost 15 Health and $10.");
          updateStats();
          hidePopup();
          shuffleButtons();
        }
      }, 1000);
    }
    
    // Modified showCustomPopup: if scenario.type is "delivery", use showDeliveryPopup.
    function showCustomPopup(scenario) {
      if (scenario.type === "delivery") {
        showDeliveryPopup(scenario);
        return;
      }
      let text = scenario.text;
      if (text.includes("[ADDRESS]")) {
        text = text.replace("[ADDRESS]", currentAddress);
      }
      document.getElementById("popupText").textContent = text;
      
      let optionsDiv = document.getElementById("popupOptions");
      optionsDiv.innerHTML = "";
      let optionsArray = scenario.options.slice();
      for (let i = optionsArray.length - 1; i > 0; i--) {
        const j = getRandomInt(0, i);
        [optionsArray[i], optionsArray[j]] = [optionsArray[j], optionsArray[i]];
      }
      optionsArray.forEach(option => {
        if (option.text === "[ADDRESS]") {
          option.text = currentAddress;
        } else if (option.text === "[WRONG]") {
          option.text = getRandomAddressExcluding(currentAddress);
        }
        let btn = document.createElement("button");
        btn.textContent = option.text;
        btn.className = "popup-option";
        btn.addEventListener("click", function() {
          clearInterval(decisionInterval);
          processPopupChoice(option);
        });
        optionsDiv.appendChild(btn);
      });
      
      decisionTime = 5;
      document.getElementById("decisionTime").textContent = decisionTime;
      document.getElementById("popupModal").style.display = "flex";
      
      decisionInterval = setInterval(function() {
        decisionTime--;
        document.getElementById("decisionTime").textContent = decisionTime;
        if (decisionTime <= 0) {
          clearInterval(decisionInterval);
          let defaultEffect = { health: -15, cash: -10, exp: 0 };
          health += defaultEffect.health;
          cash += defaultEffect.cash;
          logAction("You hesitated! Lost 15 Health and $10.");
          updateStats();
          hidePopup();
          shuffleButtons();
        }
      }, 1000);
    }
    
    // Modified showGenericPopup: if scenario.type is "delivery", use showDeliveryPopup.
    function showGenericPopup() {
      let scenario;
      if (Math.random() < 0.3) {
        scenario = popups.find(s => s.type === "goBad");
      } else if (clickCount % 2 === 0) {
        scenario = popups.find(s => s.type === "delivery");
      } else {
        scenario = popups.find(s => s.type === "activity");
        let goodOption = {
          text: goodActivities[getRandomInt(0, goodActivities.length - 1)],
          effect: getRandomGoodEffect(),
          outcome: "You did something good!",
          delivery: false
        };
        let badOption = {
          text: badActivities[getRandomInt(0, badActivities.length - 1)],
          effect: getRandomBadEffect(),
          outcome: "You did something bad!",
          delivery: false
        };
        let optionsArray = [goodOption, badOption];
        if (Math.random() < 0.5) {
          let extraOption = extraOptions[getRandomInt(0, extraOptions.length - 1)];
          optionsArray.push(extraOption);
        }
        scenario.options = optionsArray;
      }
      
      if (scenario.type === "delivery") {
        showDeliveryPopup(scenario);
        return;
      }
      
      let scenarioText = scenario.text;
      if (scenarioText.includes("[ADDRESS]")) {
        scenarioText = scenarioText.replace("[ADDRESS]", currentAddress);
      }
      document.getElementById("popupText").textContent = scenarioText;
      
      let optionsDiv = document.getElementById("popupOptions");
      optionsDiv.innerHTML = "";
      let optionsArray = scenario.options.slice();
      for (let i = optionsArray.length - 1; i > 0; i--) {
        const j = getRandomInt(0, i);
        [optionsArray[i], optionsArray[j]] = [optionsArray[j], optionsArray[i]];
      }
      optionsArray.forEach(option => {
        if (option.text === "[ADDRESS]") {
          option.text = currentAddress;
        } else if (option.text === "[WRONG]") {
          option.text = getRandomAddressExcluding(currentAddress);
        }
        let btn = document.createElement("button");
        btn.textContent = option.text;
        btn.className = "popup-option";
        btn.addEventListener("click", function() {
          clearInterval(decisionInterval);
          processPopupChoice(option);
        });
        optionsDiv.appendChild(btn);
      });
      
      decisionTime = 5;
      document.getElementById("decisionTime").textContent = decisionTime;
      document.getElementById("popupModal").style.display = "flex";
      
      decisionInterval = setInterval(function() {
        decisionTime--;
        document.getElementById("decisionTime").textContent = decisionTime;
        if (decisionTime <= 0) {
          clearInterval(decisionInterval);
          let defaultEffect = { health: -15, cash: -10, exp: 0 };
          health += defaultEffect.health;
          cash += defaultEffect.cash;
          logAction("You hesitated! Lost 15 Health and $10.");
          updateStats();
          hidePopup();
          shuffleButtons();
        }
      }, 1000);
    }
    
    function hidePopup() {
      document.getElementById("popupModal").style.display = "none";
    }
    
    function processPopupChoice(option) {
      health += option.effect.health;
      cash += option.effect.cash;
      experience += option.effect.exp;
      updateLevel();
      updateStats();
      logAction(option.outcome);
      
      if (option.delivery) {
        logAction("Delivery completed at " + currentAddress + "!");
        upcomingTasks.shift();
        upcomingTasks.push(addresses[getRandomInt(0, addresses.length - 1)]);
        updateUpcomingTasksDisplay();
        taskTimer = 30;
        currentAddress = upcomingTasks[0];
        updateObjective();
      }
      
      if (currentScenarioKey && customScenarios[currentScenarioKey] && customScenarios[currentScenarioKey].taskType) {
        let taskType = customScenarios[currentScenarioKey].taskType;
        if (taskType === "delivery" && option.delivery) {
          deliveryCount++;
          logAction("Delivery task completed! (" + deliveryCount + "/" + requiredDeliveryTasks + ")");
        } else if (taskType === "health" && option.effect.health > 0) {
          healthyCount++;
          logAction("Healthy task completed! (" + healthyCount + "/" + requiredHealthyTasks + ")");
        }
        updateStats();
        if (deliveryCount >= requiredDeliveryTasks && healthyCount >= requiredHealthyTasks) {
          level++;
          logAction("LEVEL UP! You are now level " + level + "!");
          deliveryCount = 0;
          healthyCount = 0;
          updateStats();
        }
      }
      
      hidePopup();
      shuffleButtons();
    }
    
    // UPCOMING TASKS (for deliveries)
    let upcomingTasks = [];
    function initUpcomingTasks() {
      upcomingTasks = [];
      for (let i = 0; i < 5; i++) {
        upcomingTasks.push(addresses[getRandomInt(0, addresses.length - 1)]);
      }
      updateUpcomingTasksDisplay();
      currentAddress = upcomingTasks[0];
      updateObjective();
    }
    
    function updateUpcomingTasksDisplay() {
      let div = document.getElementById("upcomingTasks");
      div.innerHTML = "<strong>Upcoming Deliveries:</strong><br>" +
                      upcomingTasks.join(", ") +
                      "<br>Delivery Task Timer: " + taskTimer + " s";
    }
    
    // Global Delivery Task Timer.
    setInterval(function() {
      if (taskTimer > 0) {
        taskTimer--;
        updateUpcomingTasksDisplay();
      } else {
        logAction("You failed to complete the delivery in time! Lost 15 Health and $10.");
        health -= 15;
        cash -= 10;
        updateStats();
        deliveryCount++;  // Count missed delivery as completed (if desired)
        upcomingTasks.shift();
        upcomingTasks.push(addresses[getRandomInt(0, addresses.length - 1)]);
        updateUpcomingTasksDisplay();
        taskTimer = 30;
        currentAddress = upcomingTasks[0];
        updateObjective();
        if (deliveryCount >= requiredDeliveryTasks && healthyCount >= requiredHealthyTasks) {
          level++;
          deliveryCount = 0;
          healthyCount = 0;
          logAction("LEVEL UP! You are now level " + level + "!");
          updateStats();
        }
      }
    }, 1000);
    
    // MAIN GAME FUNCTION
    function mainAction(actionName) {
      currentScenarioKey = actionName;
      clickCount++;
      logAction("Action chosen: " + actionName + " 🇺🇸");
      updateObjective();
      if (actionName === "Deliver Mail") {
        showCustomPopup(customScenarios["Deliver Mail"]);
      } else if (customScenarios[actionName]) {
        showCustomPopup(customScenarios[actionName]);
      } else {
        showGenericPopup();
      }
    }
    
    // BUTTON EVENT LISTENERS
    const actionButtons = document.querySelectorAll("#actions button");
    actionButtons.forEach(btn => {
      btn.addEventListener("click", function() {
        mainAction(this.textContent);
      });
    });
    
    // Global Game Timer (overall game)
    setInterval(function(){
      if (timer > 0) {
        timer--;
        updateStats();
        if (timer <= 0) {
          logAction("GAME OVER: Time's up! 🇺🇸");
          actionButtons.forEach(btn => btn.disabled = true);
        }
      }
    }, 1000);
    
    // UTILITY FUNCTIONS
    function updateStats() {
      document.getElementById("health").textContent = health;
      document.getElementById("cash").textContent = cash;
      document.getElementById("level").textContent = level;
      document.getElementById("timer").textContent = timer;
      document.getElementById("progress").textContent = "Progress: " + deliveryCount + "/" + requiredDeliveryTasks +
        " deliveries, " + healthyCount + "/" + requiredHealthyTasks + " healthy actions (" + (deliveryCount + healthyCount) + "/10 tasks)";
    }
    function updateObjective() {
      document.getElementById("currentAddress").textContent = currentAddress;
    }
    function logAction(text) {
      const li = document.createElement("li");
      li.textContent = text;
      document.getElementById("logList").appendChild(li);
      document.getElementById("log").scrollTop = document.getElementById("log").scrollHeight;
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
    
    // CUSTOM SCENARIOS
    const customScenarios = {
      "Deliver Mail": {
        type: "delivery",
        text: "Next: Deliver to [ADDRESS]. Choose the correct address:",
        taskType: "delivery",
        options: [] // Options will be generated dynamically by showDeliveryPopup.
      },
      "Drink Water": {
        text: "Time to hydrate! Choose your drink:",
        taskType: "health",
        options: [
          { text: "Drink Water", effect: { health: +5, cash: +5, exp: +2 }, outcome: "You drink water and feel refreshed!", delivery: false },
          { text: "Drink Soda", effect: { health: -5, cash: +10, exp: 0 }, outcome: "Soda tastes good but isn't as healthy.", delivery: false },
          { text: "Drink Tequila", effect: { health: -15, cash: +20, exp: -1 }, outcome: "Tequila gives a buzz but hurts your health.", delivery: false },
          { text: "Drink Beer", effect: { health: -10, cash: +15, exp: 0 }, outcome: "Beer gives a cash boost, but it's not ideal.", delivery: false }
        ]
      },
      "Take Lunch Break": {
        text: "It's lunchtime! Choose your meal:",
        taskType: "health",
        options: [
          { text: "Eat a healthy meal", effect: { health: +10, cash: 0, exp: +3 }, outcome: "A healthy meal boosts your energy!", delivery: false },
          { text: "Grab fast food", effect: { health: -5, cash: +10, exp: 0 }, outcome: "Fast food gives a cash boost but drains energy.", delivery: false },
          { text: "Skip lunch", effect: { health: -15, cash: 0, exp: -1 }, outcome: "Skipping lunch makes you weak.", delivery: false }
        ]
      },
      "Skip Delivery": {
        text: "Do you want to skip your delivery?",
        options: [
          { text: "Skip Delivery", effect: { health: -10, cash: -10, exp: -2 }, outcome: "You skipped your delivery and lost reputation.", delivery: false },
          { text: "Cancel Skip", effect: { health: +5, cash: 0, exp: +1 }, outcome: "You decide not to skip, staying on track.", delivery: false }
        ]
      },
      "Drive Carefully": {
        text: "How do you want to drive?",
        options: [
          { text: "Drive Carefully", effect: { health: +5, cash: +10, exp: +2 }, outcome: "Safe driving keeps you on schedule!", delivery: false },
          { text: "Speed", effect: { health: -10, cash: +20, exp: +1 }, outcome: "Speeding gives extra cash but is risky.", delivery: false }
        ]
      },
      "Return to Office": {
        text: "Time to return to the office. How do you do it?",
        options: [
          { text: "Return on time", effect: { health: +5, cash: +5, exp: +3 }, outcome: "You return on time and impress your boss.", delivery: false },
          { text: "Return late", effect: { health: -10, cash: -5, exp: -1 }, outcome: "Returning late incurs penalties.", delivery: false }
        ]
      },
      "Play with Dog": {
        text: "Your dog is waiting! How do you interact?",
        taskType: "health",
        options: [
          { text: "Play with Dog", effect: { health: +10, cash: +5, exp: +3 }, outcome: "You play with your dog and boost your morale!", delivery: false },
          { text: "Ignore Dog", effect: { health: -5, cash: 0, exp: -1 }, outcome: "Your dog feels neglected.", delivery: false }
        ]
      },
      "Take a Drink": {
        text: "You're offered a drink. What do you do?",
        taskType: "health",
        options: [
          { text: "Refuse Drink", effect: { health: +5, cash: 0, exp: +2 }, outcome: "You refuse the drink and stay focused.", delivery: false },
          { text: "Take Drink", effect: { health: -10, cash: +10, exp: -1 }, outcome: "You take the drink; it's fun but risky.", delivery: false },
          { text: "Chug a Shot", effect: { health: -20, cash: +20, exp: -2 }, outcome: "You chug a shot; chaos ensues.", delivery: false }
        ]
      }
    };
    
    // INITIAL SETUP
    updateStats();
    updateObjective();
    shuffleButtons();
    initUpcomingTasks();
  </script>
</body>
</html>
