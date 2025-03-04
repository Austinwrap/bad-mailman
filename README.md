<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    <meta name="apple-mobile-web-app-capable" content="yes" />
    <meta name="mobile-web-app-capable" content="yes" />
    <meta name="theme-color" content="#b22234" />
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
    <title>BAD MAILMAN - Neon Decisions 🇺🇸</title>
    <link href="https://fonts.googleapis.com/css2?family=Bubblegum+Sans&display=swap" rel="stylesheet">
    <style>
        body {
            margin: 0;
            padding: 0;
            background: linear-gradient(135deg, #b22234 0%, #ffffff 50%, #3c3b6e 100%);
            color: #fff;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            text-align: center;
            min-height: 100vh;
            background-attachment: fixed;
            background-size: 400% 400%;
            animation: gradientBG 15s ease infinite;
        }
        @keyframes gradientBG {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        #game-container {
            max-width: 800px;
            margin: 20px auto;
            padding: 20px;
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.95) 0%, rgba(178, 34, 52, 0.1) 50%, rgba(60, 59, 110, 0.2) 100%);
            border: 3px solid #b22234;
            border-radius: 10px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.7);
            position: relative;
            overflow: hidden;
        }
        h1 {
            color: #3c3b6e;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            margin-bottom: 20px;
        }
        #stats {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 8px;
            background: rgba(255, 255, 255, 0.95);
            border: 2px solid #b22234;
            padding: 10px;
            margin-bottom: 15px;
            border-radius: 5px;
            color: #001f3f;
        }
        .stat-item {
            font-size: 1.1em;
            font-weight: bold;
            color: #3c3b6e;
            text-shadow: 1px 1px 0px rgba(255,255,255,0.5);
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 5px;
            padding: 5px;
        }
        #actions {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin: 15px 0;
        }
        button {
            padding: 12px;
            font-size: 1.1em;
            border: 2px solid #b22234;
            border-radius: 8px;
            background: #b22234;
            color: #fff;
            cursor: pointer;
            transition: all 0.3s ease;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.5);
        }
        button:hover {
            background: #3c3b6e;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.3);
        }
        #modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.8);
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        #modal-content {
            background: linear-gradient(45deg, #fff, #b22234, #fff, #3c3b6e);
            padding: 25px;
            border-radius: 10px;
            max-width: 500px;
            text-align: center;
            color: #001f3f;
            animation: chaoticPulse 0.5s infinite alternate;
        }
        @keyframes chaoticPulse {
            0% { transform: scale(1) rotate(0deg); box-shadow: 0 0 10px #3c3b6e; }
            25% { transform: scale(1.1) rotate(3deg); box-shadow: 0 0 20px #ff0000; }
            50% { transform: scale(1) rotate(-3deg); box-shadow: 0 0 30px #ffffff; }
            75% { transform: scale(1.1) rotate(3deg); box-shadow: 0 0 20px #3c3b6e; }
            100% { transform: scale(1) rotate(0deg); box-shadow: 0 0 10px #b22234; }
        }
        #game-log {
            margin-top: 20px;
            padding: 10px;
            background: rgba(255, 255, 255, 0.95);
            border: 1px solid #b22234;
            border-radius: 5px;
            color: #001f3f;
            height: 150px;
            overflow-y: auto;
        }
        #upcoming-tasks {
            background: rgba(255, 255, 255, 0.95);
            border: 2px dashed #b22234;
            padding: 10px;
            margin: 15px 0;
            border-radius: 5px;
            color: #001f3f;
            cursor: pointer;
        }
        #objective {
            font-weight: bold;
            margin: 15px 0;
            color: #b22234;
        }
        #decision-timer {
            font-size: 1.2em;
            margin-top: 10px;
            color: #b22234;
            font-weight: bold;
        }
        .modal-option {
            margin: 10px;
            padding: 10px 20px;
            font-size: 1.1em;
            border: none;
            border-radius: 5px;
            background: #b22234;
            color: white;
            cursor: pointer;
            transition: all 0.3s;
        }
        .modal-option:hover {
            background: #3c3b6e;
            transform: translateY(-2px);
        }
        .tempting-choice {
            animation: pulse 1.5s infinite;
            border: 2px solid gold !important;
            box-shadow: 0 0 15px rgba(255, 215, 0, 0.5);
        }
        @keyframes pulse {
            0% { transform: scale(1); box-shadow: 0 0 15px rgba(255, 215, 0, 0.5); }
            50% { transform: scale(1.05); box-shadow: 0 0 25px rgba(255, 215, 0, 0.8); }
            100% { transform: scale(1); box-shadow: 0 0 15px rgba(255, 215, 0, 0.5); }
        }
        .shake-level1 {
            animation: shakeLevel1 0.3s infinite;
        }
        .shake-level2 {
            animation: shakeLevel2 0.15s infinite;
        }
        .shake-level3 {
            animation: shakeLevel3 0.05s infinite;
        }
        @keyframes shakeLevel1 {
            0% { transform: translate(0.5px, 0.5px) rotate(0deg); }
            25% { transform: translate(-0.5px, -0.5px) rotate(-0.5deg); }
            50% { transform: translate(-0.5px, 0.5px) rotate(0.5deg); }
            75% { transform: translate(0.5px, -0.5px) rotate(-0.5deg); }
            100% { transform: translate(0.5px, 0.5px) rotate(0deg); }
        }
        @keyframes shakeLevel2 {
            0% { transform: translate(1px, 1px) rotate(0deg); }
            25% { transform: translate(-1px, -1px) rotate(-1deg); }
            50% { transform: translate(-1px, 1px) rotate(1deg); }
            75% { transform: translate(1px, -1px) rotate(-1deg); }
            100% { transform: translate(1px, 1px) rotate(0deg); }
        }
        @keyframes shakeLevel3 {
            0% { transform: translate(2px, 2px) rotate(0deg); }
            25% { transform: translate(-2px, -2px) rotate(-2deg); }
            50% { transform: translate(-2px, 2px) rotate(2deg); }
            75% { transform: translate(2px, -2px) rotate(-2deg); }
            100% { transform: translate(2px, 2px) rotate(0deg); }
        }
        .blurred-text {
            filter: blur(2px);
            transition: filter 0.3s;
        }
        .blurred-text:hover {
            filter: blur(0);
        }
        button.shake-level1, button.shake-level2, button.shake-level3 {
            pointer-events: auto;
        }
        .endgame-text {
            font-family: 'Bubblegum Sans', cursive;
            font-size: 1.5em;
            color: #b22234;
            text-shadow: 2px 2px 4px #ffffff;
        }
        .endgame-title {
            font-family: 'Bubblegum Sans', cursive;
            font-size: 2.5em;
            color: #3c3b6e;
            text-shadow: 2px 2px 6px #ff0000;
        }
    </style>
</head>
<body>
    <div id="game-container">
        <h1>BAD MAILMAN 🇺🇸</h1>
        <div id="main-menu">
            <button onclick="showStartMenu()">Start Game</button>
        </div>
        <div id="game-content" style="display: none;">
            <div id="stats">
                <div class="stat-item">Health: <span id="health">100</span></div>
                <div class="stat-item">Cash: $<span id="cash">50</span></div>
                <div class="stat-item">Level: <span id="level">1</span>/3</div>
                <div class="stat-item">Badness: <span id="badness">0</span></div>
                <div class="stat-item">Time: <span id="timer">300</span>s</div>
            </div>
            <div id="objective">
                <h3>Current Mission:</h3>
                <p>Next Delivery: <span id="current-address"></span> 🇺🇸</p>
                <p>Deliveries: <span id="deliveries">0</span>/<span id="required-deliveries">3</span></p>
                <p>Tasks: <span id="tasks">0</span>/<span id="required-tasks">3</span></p>
            </div>
            <div id="upcoming-tasks">
                <h3>Delivery Information</h3>
                <p>Current Delivery: <span id="current-address-display"></span></p>
                <p>Time for delivery: <span id="task-timer">45</span>s</p>
                <p class="hidden-list" id="upcoming-list"></p>
                <p style="font-style: italic; color: #666;">Click to check delivery list</p>
            </div>
            <div id="actions"></div>
            <div id="game-log"></div>
        </div>
    </div>
    <div id="modal">
        <div id="modal-content">
            <p id="modal-text"></p>
            <div id="modal-options"></div>
            <div id="decision-timer">Time: <span id="decision-time">12</span>s</div>
        </div>
    </div>

    <script>
        const ADDRESSES = [
            "42 Irving St", "42 Irving Dr", "42 Irving Ave", "42 Irving Ct",
            "44 Irving St", "44 Irving Dr", "44 Irving Ave", "44 Irving Ct",
            "46 Irving St", "46 Irving Dr", "46 Irving Ave", "46 Irving Ct",
            "101 Washington St", "101 Washington Ave", "101 Washington Blvd", "101 Washington Dr",
            "103 Washington St", "103 Washington Ave", "103 Washington Blvd", "103 Washington Dr",
            "220 Oak St", "220 Oak Ave", "220 Oak Ln", "220 Oak Circle",
            "222 Oak St", "222 Oak Ave", "222 Oak Ln", "222 Oak Circle",
            "335 Maple Dr", "335 Maple St", "335 Maple Ave", "335 Maple Ct",
            "337 Maple Dr", "337 Maple St", "337 Maple Ave", "337 Maple Ct"
        ];

        const ORIGIN_STORY = [
            "You were once a dedicated mailman, loved by your community.",
            "Then the economy crashed. Your pension vanished.",
            "Your wife left, taking the kids and your hope.",
            "The drinking started small - just to numb the pain.",
            "The local gangs offered quick cash for 'misplacing' packages.",
            "Now you're in too deep. Each delivery is a choice.",
            "Will you crawl back to dignity or embrace the corruption?"
        ];

        const TEMPTATIONS = [
            {
                text: "A shady character offers you a bottle of whiskey...",
                options: [
                    { text: "Drink it (+Energy, -Health)", effect: { health: -15, cash: 0, badness: 20, type: "bad" }, outcome: "The world spins a little...", tempting: true },
                    { text: "Sell it (+Cash)", effect: { health: 0, cash: 50, badness: 10, type: "bad" }, outcome: "Quick cash, less guilt.", tempting: true },
                    { text: "Refuse", effect: { health: 5, cash: 0, type: "good" }, outcome: "You stay sober." }
                ]
            },
            {
                text: "You find some 'special pills' in a package...",
                options: [
                    { text: "Take them (+Energy, -Health)", effect: { health: -25, cash: 0, badness: 30, type: "bad" }, outcome: "Reality blurs...", tempting: true },
                    { text: "Sell them (+Big Cash)", effect: { health: -10, cash: 100, badness: 20, type: "bad" }, outcome: "Profit over morals.", tempting: true },
                    { text: "Destroy them", effect: { health: 10, cash: 0, type: "good" }, outcome: "A rare good deed." }
                ]
            }
        ];

        const CHAOS_EVENTS = [
            {
                text: "Your mail truck's brakes feel loose on a hill...",
                options: [
                    { text: "Crash into cars (+Cash)", effect: { health: -30, cash: 200, badness: 15, type: "bad" }, outcome: "The 'accident' pays...", tempting: true },
                    { text: "Risk it", effect: { health: -20, cash: 0, badness: 10, type: "bad" }, outcome: "Adrenaline rush!" },
                    { text: "Report it", effect: { health: 5, cash: -20, type: "good" }, outcome: "Safety first." }
                ]
            }
        ];

        let gameState = {
            health: 100,
            cash: 50,
            level: 1,
            badness: 0,
            timer: 300,
            deliveries: 0,
            tasks: 0,
            gameInterval: null,
            taskTimer: 45,
            taskInterval: null,
            decisionTimer: null,
            currentAddress: "",
            upcomingDeliveries: [],
            storyIndex: 0,
            badChoices: 0,
            goodChoices: 0,
            decisionTime: 12,
            showOriginStory: false,
            lastDogInteraction: 0,
            dogInteractions: 0,
            usedAddresses: new Set(),
            currentLevelDeliveries: [],
            completedTaskTypes: new Set(),
            deliveryOptions: new Set(),
            character: "",
            isUnderInfluence: false
        };

        const LEVELS = {
            1: { deliveries: 3, tasks: 3, timeBonus: 120 },
            2: { deliveries: 3, tasks: 3, timeBonus: 150 },
            3: { deliveries: 3, tasks: 3, timeBonus: 180 }
        };

        const LEVEL_BUTTONS = {
            1: [
                { action: 'deliver', text: 'Deliver Mail 🇺🇸' },
                { action: 'water', text: 'Drink Water (+Health)' },
                { action: 'lunch', text: 'Take Lunch (+Health)' },
                { action: 'skip', text: 'Skip Delivery' },
                { action: 'dog', text: 'Play with Dog (+Task)' },
                { action: 'return', text: 'Return to Office' },
                { action: 'rest', text: 'Take Break (+Health)' },
                { action: 'organize', text: 'Organize Bag (+Task)' }
            ],
            2: [
                { action: 'deliver', text: 'Deliver Mail 🇺🇸' },
                { action: 'drink', text: 'Drink Whiskey (-Health)' },
                { action: 'water', text: 'Drink Water (+Health)' },
                { action: 'dog', text: 'Play with Dog (+Task)' },
                { action: 'skip', text: 'Skip Delivery' },
                { action: 'lunch', text: 'Take Lunch (+Health)' },
                { action: 'steal', text: 'Steal Package (+Cash)' },
                { action: 'exercise', text: 'Quick Exercise (+Health)' }
            ],
            3: [
                { action: 'deliver', text: 'Deliver Mail 🇺🇸' },
                { action: 'drugs', text: 'Take Pills (-Health, +Cash)' },
                { action: 'drink', text: 'Drink Whiskey (-Health)' },
                { action: 'dog', text: 'Play with Dog (+Task)' },
                { action: 'skip', text: 'Skip Delivery' },
                { action: 'lunch', text: 'Take Lunch (+Health)' },
                { action: 'scam', text: 'Run Mail Scam (+Big Cash)' },
                { action: 'exercise', text: 'Quick Exercise (+Health)' }
            ]
        };

        function showStartMenu() {
            const mainMenu = document.getElementById("main-menu");
            mainMenu.innerHTML = `
                <h1 class="welcome-title">BAD MAILMAN 🇺🇸</h1>
                <div class="welcome-message">
                    <p>⏱️ 3 INTENSE LEVELS - EACH TIMED!</p>
                    <p>🎯 Deliver mail, stay alive, earn cash</p>
                    <p>⚠️ Temptation lurks...</p>
                </div>
            `;
            showTimedChoice(
                "Ready to begin?",
                [
                    { text: "Start Game ⚡", effect: { type: "character" }, outcome: "Choosing character..." },
                    { text: "Origin Story 📖", effect: { type: "origin" }, outcome: "Loading story..." }
                ],
                12,
                (choice) => {
                    if (choice.effect.type === "origin") {
                        gameState.showOriginStory = true;
                        startOriginStory();
                    } else {
                        showCharacterSelection();
                    }
                }
            );
        }

        function startOriginStory() {
            gameState.storyIndex = 0;
            showModal(ORIGIN_STORY[0], [{ text: "Next", action: nextStory }]);
        }

        function nextStory() {
            gameState.storyIndex++;
            if (gameState.storyIndex < ORIGIN_STORY.length) {
                showModal(ORIGIN_STORY[gameState.storyIndex], [{ text: "Next", action: nextStory }]);
            } else {
                hideModal();
                startGame();
            }
        }

        function startGame() {
            cleanupGameState();
            resetGameState();
            document.getElementById("main-menu").style.display = "none";
            document.getElementById("game-content").style.display = "block";
            initializeDeliveries();
            showTimedChoice(
                "📬 WELCOME TO BAD MAILMAN!\n\nYour goal: Make 3 deliveries and complete 3 tasks across 3 timed rounds.\n\nYou're trying to uphold the Mailman Creed, but old habits keep pulling you back...\n\nStay sharp, time's ticking!",
                [{ text: "Let's Go!", effect: { type: "start" }, outcome: "Game on!" }],
                20,
                () => {
                    hideModal();
                    showTimedChoice(
                        `🚨 MEMORIZE DELIVERIES!\n\n${gameState.upcomingDeliveries.join(" → ")}\n\n20s to memorize.\nLevel 1: List visible\nLevel 2-3: Pay to check`,
                        [{ text: "I'm Ready!", effect: { type: "start" }, outcome: "Starting..." }],
                        20,
                        () => {
                            hideModal();
                            startTimers();
                            updateUI();
                            updateButtons();
                            updateDeliveryListVisibility();
                            addToLog("Game started!");
                            setTimeout(showTemptation, 10000);
                        }
                    );
                }
            );
        }

        function cleanupGameState() {
            if (gameState.gameInterval) clearInterval(gameState.gameInterval);
            if (gameState.taskInterval) clearInterval(gameState.taskInterval);
            if (gameState.decisionTimer) clearInterval(gameState.decisionTimer);
            hideModal();
            document.getElementById("game-content").style.display = "none";
            document.getElementById("main-menu").style.display = "block";
            document.getElementById("game-log").innerHTML = "";
        }

        function resetGameState() {
            gameState = {
                health: 100,
                cash: 50,
                level: 1,
                badness: 0,
                timer: 300,
                deliveries: 0,
                tasks: 0,
                gameInterval: null,
                taskTimer: 45,
                taskInterval: null,
                decisionTimer: null,
                currentAddress: "",
                upcomingDeliveries: [],
                storyIndex: 0,
                badChoices: 0,
                goodChoices: 0,
                decisionTime: 12,
                showOriginStory: false,
                lastDogInteraction: 0,
                dogInteractions: 0,
                usedAddresses: new Set(),
                currentLevelDeliveries: [],
                completedTaskTypes: new Set(),
                deliveryOptions: new Set(),
                character: gameState.character,
                isUnderInfluence: false
            };
        }

        function initializeDeliveries() {
            gameState.usedAddresses = new Set();
            gameState.currentLevelDeliveries = [];
            let availableAddresses = ADDRESSES.filter(addr => !gameState.usedAddresses.has(addr));
            if (availableAddresses.length < 5) {
                gameState.usedAddresses.clear();
                availableAddresses = [...ADDRESSES];
            }
            for (let i = 0; i < 5; i++) {
                const randomIndex = Math.floor(Math.random() * availableAddresses.length);
                const selectedAddress = availableAddresses[randomIndex];
                gameState.currentLevelDeliveries.push(selectedAddress);
                gameState.usedAddresses.add(selectedAddress);
                availableAddresses.splice(randomIndex, 1);
            }
            gameState.currentAddress = gameState.currentLevelDeliveries[0];
            updateUpcomingDeliveries();
        }

        function startTimers() {
            if (gameState.gameInterval) clearInterval(gameState.gameInterval);
            if (gameState.taskInterval) clearInterval(gameState.taskInterval);
            gameState.timer = 300 + (gameState.level * 60);
            gameState.gameInterval = setInterval(() => {
                gameState.timer--;
                if (gameState.timer <= 0) endGame("⏰ Time's up!");
                if (gameState.health <= 0) endGame("💔 You collapsed!");
                updateUI();
            }, 1000);
            gameState.taskTimer = 60;
            gameState.taskInterval = setInterval(() => {
                gameState.taskTimer--;
                if (gameState.taskTimer <= 0) failDelivery();
                updateUI();
            }, 1000);
        }

        function performAction(actionType) {
            if (document.getElementById("modal").style.display === "flex") return;
            switch (actionType) {
                case 'deliver': showDeliveryChallenge(); break;
                case 'dog': handleDogAction(); break;
                case 'drink': handleDrinkAction(); break;
                case 'drugs': handleDrugsAction(); break;
                case 'skip': handleSkipAction(); break;
                case 'return': handleReturnAction(); break;
                default: handleDefaultAction(actionType); break;
            }
        }

        function showDeliveryChallenge() {
            if (!gameState.currentAddress) {
                addToLog("Error: No current address!");
                return;
            }
            const correctAddress = gameState.currentAddress;
            const [number, ...streetParts] = correctAddress.split(" ");
            const streetBase = streetParts.slice(0, -1).join(" ");
            const streetType = streetParts[streetParts.length - 1];
            gameState.deliveryOptions.clear(); // Reset options
            const similarAddresses = [];
            while (similarAddresses.length < 3) {
                const newAddress = generateUniqueAddress(number, streetBase, streetType);
                if (!gameState.deliveryOptions.has(newAddress) && newAddress !== correctAddress) {
                    similarAddresses.push(newAddress);
                    gameState.deliveryOptions.add(newAddress);
                }
            }
            // Explicitly include the correct address
            const baseReward = 80 + (gameState.level * 50);
            const options = [
                { text: correctAddress, effect: { health: 5, cash: 15, type: "delivery" }, outcome: "Perfect delivery!" },
                ...similarAddresses.map(addr => ({
                    text: addr,
                    effect: { health: -10, cash: -10, type: "none" },
                    outcome: "Wrong address!"
                })),
                { text: `💰 "Lose" package (+$${baseReward})`, effect: { health: -15, cash: baseReward, badness: 15, type: "bad" }, outcome: "Easy money...", tempting: true },
                { text: `💎 Sell contents (+$${baseReward + 50})`, effect: { health: -20, cash: baseReward + 50, badness: 20, type: "bad" }, outcome: "Profit!", tempting: true }
            ];
            // Debug check
            if (!options.some(opt => opt.text === correctAddress)) {
                console.error(`Correct address ${correctAddress} not in options!`, options);
                addToLog("Error: Correct address missing!");
            }
            showTimedChoice(
                `📬 DELIVER\n✅ Correct: +5 Health, +$15\n❌ Wrong: -10 Health, -$10\n💰 Criminal: More Cash, Less Soul`,
                shuffleArray(options),
                20,
                handleDeliveryChoice
            );
        }

        function handleDeliveryChoice(choice) {
            vibrateDevice(100);
            clearInterval(gameState.decisionTimer);
            gameState.health += choice.effect.health;
            gameState.cash += choice.effect.cash;
            gameState.badness += choice.effect.badness || 0;
            gameState.badness = Math.min(100, Math.max(0, gameState.badness));
            if (choice.effect.type === "delivery") {
                gameState.deliveries++;
                addToLog(`Delivery completed! (${gameState.deliveries}/${LEVELS[gameState.level].deliveries})`);
                if (Math.random() < 0.8) setTimeout(showTemptation, 600);
                if (checkProgress()) return;
            }
            if (choice.effect.type === "bad") {
                gameState.badChoices++;
                if (Math.random() < 0.6) setTimeout(showTemptation, 1000);
            }
            addToLog(choice.outcome);
            hideModal();
            gameState.taskTimer = 45;
            gameState.currentLevelDeliveries.shift();
            if (gameState.currentLevelDeliveries.length > 0) gameState.currentAddress = gameState.currentLevelDeliveries[0];
            else gameState.currentAddress = "";
            updateUpcomingDeliveries();
            updateUI();
            saveGameState();
        }

        function checkProgress() {
            const levelConfig = LEVELS[gameState.level];
            if (gameState.deliveries >= levelConfig.deliveries && gameState.tasks >= levelConfig.tasks) {
                if (gameState.gameInterval) clearInterval(gameState.gameInterval);
                if (gameState.taskInterval) clearInterval(gameState.taskInterval);
                if (gameState.decisionTimer) clearInterval(gameState.decisionTimer);
                if (gameState.level < 3) {
                    addToLog("🎉 Level completed!");
                    levelUp();
                    return true;
                } else {
                    addToLog("🎉 Game mastered!");
                    showTimedChoice(
                        "A figure offers you the postal empire...",
                        [
                            { text: "Accept (+$1000, -50 Health)", effect: { health: -50, cash: 1000, badness: 30, type: "bad" }, outcome: "Darkness reigns.", tempting: true },
                            { text: "Decline", effect: { health: 20, cash: 50, type: "good" }, outcome: "Integrity intact." }
                        ],
                        20,
                        (choice) => {
                            gameState.health += choice.effect.health;
                            gameState.cash += choice.effect.cash;
                            gameState.badness += choice.effect.badness || 0;
                            addToLog(choice.outcome);
                            endGame("🎉 Game Complete!");
                        }
                    );
                    return true;
                }
            }
            return false;
        }

        function levelUp() {
            gameState.level++;
            gameState.deliveries = 0;
            gameState.tasks = 0;
            gameState.completedTaskTypes = new Set();
            gameState.timer += LEVELS[gameState.level].timeBonus;
            initializeDeliveries();
            updateUI();
            showTimedChoice(
                `🎉 LEVEL ${gameState.level} UNLOCKED!\n\nNew Deliveries:\n${gameState.currentLevelDeliveries.join(" → ")}\n\nTime Bonus: +${LEVELS[gameState.level].timeBonus}s`,
                [{ text: `Start Level ${gameState.level}!`, effect: { type: "start" }, outcome: "Starting..." }],
                25,
                () => {
                    hideModal();
                    startTimers();
                    updateButtons();
                    updateDeliveryListVisibility();
                    addToLog(`📈 Advanced to Level ${gameState.level}!`);
                    updateUI();
                    setTimeout(showTemptation, 8000);
                }
            );
        }

        function endGame(message) {
            cleanupGameState();
            const finalScore = Math.floor(gameState.cash + (gameState.health * 2));
            const endingType = gameState.badChoices > gameState.goodChoices ? "CORRUPTED" : "RIGHTEOUS";
            showModal(
                `<h2 class="endgame-title">BAD MAILMAN 🇺🇸</h2>
                <p class="endgame-text">${message}</p>
                <p class="endgame-text">📊 Your Results:</p>
                <p class="endgame-text">Character: ${gameState.character}</p>
                <p class="endgame-text">Ending: ${endingType}</p>
                <p class="endgame-text">Level Reached: ${gameState.level}/3</p>
                <p class="endgame-text">Final Score: ${finalScore}</p>
                <p class="endgame-text">Health: ${Math.floor(gameState.health)}</p>
                <p class="endgame-text">Cash: $${gameState.cash}</p>
                <p class="endgame-text">Bad Choices: ${gameState.badChoices}</p>
                <p class="endgame-text">Good Choices: ${gameState.goodChoices}</p>
                <p class="endgame-text">Badness: ${gameState.badness}</p>
                <p class="endgame-text">🌟 Dare to be the BADDEST Mailman? Play Again! 🌟</p>`,
                [{ text: "PLAY AGAIN!", action: () => { hideModal(); showStartMenu(); } }]
            );
        }

        function showModal(message, options) {
            const modal = document.getElementById("modal");
            const modalText = document.getElementById("modal-text");
            const modalOptions = document.getElementById("modal-options");
            modalText.innerHTML = message;
            modalOptions.innerHTML = "";
            options.forEach(option => {
                const button = document.createElement("button");
                button.className = "modal-option";
                button.textContent = option.text;
                button.onclick = option.action;
                modalOptions.appendChild(button);
            });
            modal.style.display = "flex";
        }

        function hideModal() {
            document.getElementById("modal").style.display = "none";
        }

        function updateUI() {
            document.getElementById("health").textContent = Math.floor(gameState.health);
            document.getElementById("cash").textContent = gameState.cash;
            document.getElementById("level").textContent = gameState.level;
            document.getElementById("badness").textContent = gameState.badness;
            document.getElementById("timer").textContent = gameState.timer;
            document.getElementById("deliveries").textContent = gameState.deliveries;
            document.getElementById("tasks").textContent = gameState.tasks;
            document.getElementById("current-address").textContent = gameState.currentAddress || "No delivery";
            document.getElementById("current-address-display").textContent = gameState.currentAddress || "No delivery";
            document.getElementById("task-timer").textContent = gameState.taskTimer;
            document.getElementById("required-deliveries").textContent = LEVELS[gameState.level].deliveries;
            document.getElementById("required-tasks").textContent = LEVELS[gameState.level].tasks;
            updateButtonShake();
        }

        function updateButtonShake() {
            const buttons = document.querySelectorAll("#actions button, #modal-options button");
            buttons.forEach(button => {
                button.classList.remove("shake-level1", "shake-level2", "shake-level3");
                if (gameState.isUnderInfluence) {
                    if (gameState.level === 1 && gameState.badness >= 25) button.classList.add("shake-level1");
                    else if (gameState.level === 2 && gameState.badness >= 25) button.classList.add("shake-level2");
                    else if (gameState.level === 3 && gameState.badness >= 25) button.classList.add("shake-level3");
                }
            });
        }

        function updateUpcomingDeliveries() {
            document.getElementById("upcoming-list").textContent = gameState.currentLevelDeliveries.join(" → ");
        }

        function addToLog(message) {
            const log = document.getElementById("game-log");
            const entry = document.createElement("div");
            entry.className = "log-entry";
            entry.textContent = message;
            log.insertBefore(entry, log.firstChild);
        }

        function updateButtons() {
            const actionsDiv = document.getElementById("actions");
            actionsDiv.innerHTML = "";
            const buttons = shuffleArray([...LEVEL_BUTTONS[gameState.level]]);
            while (buttons.length < 8) buttons.push(buttons[Math.floor(Math.random() * buttons.length)]);
            buttons.forEach(button => {
                const btn = document.createElement("button");
                btn.textContent = button.text;
                btn.onclick = () => performAction(button.action);
                actionsDiv.appendChild(btn);
            });
            updateButtonShake();
        }

        function showTimedChoice(text, options, time, callback) {
            if (gameState.decisionTimer) clearInterval(gameState.decisionTimer);
            const modal = document.getElementById("modal");
            const modalText = document.getElementById("modal-text");
            const modalOptions = document.getElementById("modal-options");
            const timerDisplay = document.getElementById("decision-time");
            modalText.textContent = text;
            modalOptions.innerHTML = "";
            if (gameState.isUnderInfluence && gameState.badness >= 50) {
                modalText.classList.add("blurred-text");
                modalOptions.classList.add("blurred-text");
            } else {
                modalText.classList.remove("blurred-text");
                modalOptions.classList.remove("blurred-text");
            }
            options.forEach(option => {
                const button = document.createElement("button");
                button.className = "modal-option";
                if (option.tempting) button.classList.add("tempting-choice");
                button.textContent = option.text;
                button.onclick = () => {
                    clearInterval(gameState.decisionTimer);
                    gameState.decisionTimer = null;
                    callback(option);
                };
                modalOptions.appendChild(button);
            });
            modal.style.display = "flex";
            let timeLeft = time;
            timerDisplay.textContent = timeLeft;
            gameState.decisionTimer = setInterval(() => {
                timeLeft--;
                timerDisplay.textContent = timeLeft;
                if (timeLeft <= 0) {
                    clearInterval(gameState.decisionTimer);
                    gameState.decisionTimer = null;
                    handleTimeout();
                }
            }, 1000);
            updateButtonShake();
        }

        function handleTimeout() {
            gameState.health -= 15;
            gameState.cash -= 10;
            if (gameState.health === 100 && gameState.cash === 50) {
                gameState.showOriginStory = true;
                startOriginStory();
            } else {
                addToLog("You hesitated! Lost 15 Health and $10.");
                hideModal();
                updateUI();
            }
        }

        function handleTemptationChoice(choice) {
            clearInterval(gameState.decisionTimer);
            gameState.health += choice.effect.health;
            gameState.cash += choice.effect.cash || 0;
            gameState.badness += choice.effect.badness || 0;
            gameState.badness = Math.min(100, Math.max(0, gameState.badness));
            if (choice.effect.type === "bad") {
                gameState.badChoices++;
                gameState.isUnderInfluence = choice.effect.badness > 0;
                if (gameState.isUnderInfluence) {
                    setTimeout(() => {
                        gameState.isUnderInfluence = false;
                        addToLog("The buzz wears off...");
                        updateButtonShake();
                    }, 10000);
                }
            } else {
                gameState.goodChoices++;
            }
            addToLog(choice.outcome);
            hideModal();
            updateUI();
        }

        function showTemptation() {
            if (document.getElementById("modal").style.display === "flex") return;
            const temptation = TEMPTATIONS[Math.floor(Math.random() * TEMPTATIONS.length)];
            showTimedChoice(temptation.text, shuffleArray(temptation.options), 20, handleTemptationChoice);
        }

        function shuffleArray(array) {
            const newArray = [...array];
            for (let i = newArray.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [newArray[i], newArray[j]] = [newArray[j], newArray[i]];
            }
            return newArray;
        }

        function failDelivery() {
            gameState.health -= 15;
            gameState.cash -= 10;
            addToLog("Delivery failed! Lost 15 Health and $10");
            gameState.taskTimer = 45;
            gameState.currentLevelDeliveries.shift();
            if (gameState.currentLevelDeliveries.length > 0) gameState.currentAddress = gameState.currentLevelDeliveries[0];
            else gameState.currentAddress = "";
            updateUpcomingDeliveries();
            updateUI();
            saveGameState();
        }

        function handleDogAction() {
            const currentTime = Date.now();
            if (currentTime - gameState.lastDogInteraction < 10000 && gameState.lastDogInteraction !== 0) {
                addToLog("The dog needs a break!");
                return;
            }
            if (gameState.completedTaskTypes.has('dog')) {
                showTimedChoice(
                    "You've bonded with this dog today...",
                    [
                        { text: "Train for crime (+Cash)", effect: { health: -20, cash: 50, badness: 15, type: "bad" }, outcome: "Dog turns criminal...", tempting: true },
                        { text: "Just play (+Health)", effect: { health: 5, cash: 0, type: "none" }, outcome: "A simple joy." }
                    ],
                    gameState.decisionTime,
                    handleTemptationChoice
                );
                return;
            }
            gameState.completedTaskTypes.add('dog');
            gameState.lastDogInteraction = currentTime;
            gameState.dogInteractions++;
            gameState.health += 10;
            gameState.tasks++;
            addToLog(`Played with dog: +10 Health, +1 Task.`);
            updateUI();
        }

        function handleDrinkAction() {
            gameState.health -= 20;
            gameState.badness += 15;
            gameState.isUnderInfluence = true;
            addToLog("You drank whiskey... Everything shakes.");
            setTimeout(() => {
                gameState.isUnderInfluence = false;
                addToLog("The buzz fades...");
                updateButtonShake();
            }, 10000);
            updateUI();
            if (Math.random() < 0.5) setTimeout(showTemptation, 1000);
        }

        function handleDrugsAction() {
            gameState.health -= 30;
            gameState.cash += 50;
            gameState.badness += 25;
            gameState.isUnderInfluence = true;
            addToLog("You took pills... Reality blurs.");
            setTimeout(() => {
                gameState.isUnderInfluence = false;
                addToLog("The high fades...");
                updateButtonShake();
            }, 15000);
            updateUI();
            if (Math.random() < 0.6) setTimeout(showTemptation, 1000);
        }

        function handleSkipAction() {
            const skipPenalty = 15 + (gameState.level * 5);
            gameState.health -= 10;
            gameState.cash -= skipPenalty;
            addToLog(`Skipped delivery: -$${skipPenalty}, -10 Health.`);
            gameState.taskTimer = 45;
            gameState.currentLevelDeliveries.shift();
            if (gameState.currentLevelDeliveries.length > 0) gameState.currentAddress = gameState.currentLevelDeliveries[0];
            else gameState.currentAddress = "";
            updateUpcomingDeliveries();
            updateUI();
        }

        function handleReturnAction() {
            const completionPercent = ((gameState.deliveries / LEVELS[gameState.level].deliveries) + (gameState.tasks / LEVELS[gameState.level].tasks)) * 50;
            showTimedChoice(
                `Return early? (${Math.floor(completionPercent)}% complete)\nEnds shift now!`,
                [
                    { text: "Return", effect: { type: "return" }, outcome: "Shift ended..." },
                    { text: "Keep working", effect: { type: "continue" }, outcome: "Back to work..." }
                ],
                gameState.decisionTime,
                (choice) => {
                    if (choice.effect.type === "return") {
                        gameState.cash = Math.floor(gameState.cash * (completionPercent / 100));
                        gameState.health -= 30;
                        endGame("Returned early. Management disappointed...");
                    }
                    hideModal();
                }
            );
        }

        function handleDefaultAction(actionType) {
            const effects = {
                water: { health: 10, cash: 0, type: "task" },
                lunch: { health: 15, cash: -5, type: "task" },
                rest: { health: 20, cash: 0, type: "task" },
                organize: { health: 5, cash: 5, type: "task" },
                exercise: { health: 15, cash: -5, type: "task" },
                steal: { health: -20, cash: 30, badness: 10, type: "bad" },
                scam: { health: -25, cash: 50, badness: 15, type: "bad" }
            };
            const action = effects[actionType];
            if (!action) return;
            gameState.health += action.health;
            gameState.cash += action.cash;
            gameState.badness += action.badness || 0;
            if (action.type === "task" && !gameState.completedTaskTypes.has(actionType)) {
                gameState.tasks++;
                gameState.completedTaskTypes.add(actionType);
                addToLog(`Task completed! (${gameState.tasks}/${LEVELS[gameState.level].tasks})`);
                if (checkProgress()) return;
            }
            addToLog(`${actionType}: Health ${action.health > 0 ? "+" : ""}${action.health}, Cash ${action.cash > 0 ? "+" : ""}$${action.cash}`);
            updateUI();
            saveGameState();
        }

        function updateDeliveryListVisibility() {
            const upcomingTasks = document.getElementById("upcoming-tasks");
            if (gameState.level === 1) {
                upcomingTasks.onclick = () => {
                    showModal(`📝 Delivery List:\n\n${gameState.currentLevelDeliveries.join(" → ")}`, [{ text: "Got it!", action: hideModal }]);
                };
            } else {
                upcomingTasks.onclick = () => {
                    const penalty = 10 + (gameState.level * 5);
                    showTimedChoice(
                        `⚠️ Check list?\n-$${penalty}\n-5 Health`,
                        [
                            { text: "Yes", effect: { type: "show", health: -5, cash: -penalty }, outcome: "Checking..." },
                            { text: "No", effect: { type: "cancel" }, outcome: "Cancelled" }
                        ],
                        gameState.decisionTime,
                        (choice) => {
                            if (choice.effect.type === "show") {
                                gameState.health += choice.effect.health;
                                gameState.cash += choice.effect.cash;
                                showModal(`📝 DELIVERY LIST:\n\n${gameState.currentLevelDeliveries.join(" → ")}`, [{ text: "Memorized", action: hideModal }]);
                                addToLog(`Checked list: -$${penalty}, -5 Health`);
                                updateUI();
                            }
                            hideModal();
                        }
                    );
                };
            }
        }

        function showCharacterSelection() {
            const characters = [
                { name: "Andrew", emoji: "👨🏼" },
                { name: "Dario", emoji: "👨🏾" },
                { name: "Raeanne", emoji: "👩🏼" },
                { name: "Wilbur", emoji: "👨🏿" }
            ];
            showModal(
                "👤 SELECT CHARACTER",
                characters.map(char => ({
                    text: `${char.emoji} ${char.name}`,
                    action: () => {
                        gameState.character = char.name;
                        hideModal();
                        if (gameState.showOriginStory) startOriginStory();
                        else startGame();
                        addToLog(`Playing as ${char.name}`);
                    }
                }))
            );
        }

        function generateUniqueAddress(number, streetBase, currentType) {
            const types = ['St', 'Ave', 'Dr', 'Ct', 'Ln', 'Circle', 'Blvd', 'Place', 'Road'];
            let address;
            do {
                const randomType = types[Math.floor(Math.random() * types.length)];
                const numberMod = Math.random() < 0.5 ? parseInt(number) + 2 : parseInt(number) - 2;
                address = `${numberMod} ${streetBase} ${randomType}`;
            } while (gameState.deliveryOptions.has(address));
            return address;
        }

        function vibrateDevice(duration) {
            if ('vibrate' in navigator) navigator.vibrate(duration);
        }

        function saveGameState() {
            try {
                localStorage.setItem('badMailmanState', JSON.stringify(gameState));
            } catch (e) {
                console.error('Failed to save:', e);
            }
        }

        function loadGameState() {
            try {
                const saved = localStorage.getItem('badMailmanState');
                if (saved) {
                    gameState = JSON.parse(saved);
                    updateUI();
                }
            } catch (e) {
                console.error('Failed to load:', e);
            }
        }

        window.onload = function() {
            document.body.style.overflow = 'auto';
            document.body.style.position = 'relative';
            loadGameState();
            showStartMenu();
        };
    </script>
</body>
</html>
