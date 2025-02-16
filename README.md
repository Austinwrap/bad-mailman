<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    <meta name="apple-mobile-web-app-capable" content="yes" />
    <meta name="mobile-web-app-capable" content="yes" />
    <meta name="theme-color" content="#b22234" />
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
    <title>BAD MAILMAN - Neon Decisions 🇺🇸</title>
    <style>
        /* American Flag Red, White & Blue Theme */
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
        #game-container::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: 
                linear-gradient(45deg, 
                    rgba(178, 34, 52, 0.1) 0%,
                    rgba(255, 255, 255, 0.2) 50%,
                    rgba(60, 59, 110, 0.1) 100%
                );
            pointer-events: none;
            z-index: 1;
        }
        h1 {
            color: #3c3b6e;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            margin-bottom: 20px;
        }
        #stats {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            background: rgba(255, 255, 255, 0.95);
            border: 2px solid #b22234;
            padding: 15px;
            margin-bottom: 15px;
            border-radius: 5px;
            color: #001f3f;
        }
        .stat-item {
            font-size: 1.2em;
            font-weight: bold;
            color: #3c3b6e;
            text-shadow: 1px 1px 0px rgba(255,255,255,0.5);
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
        button:disabled {
            background: #888;
            cursor: not-allowed;
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
        .log-entry {
            margin: 5px 0;
            padding: 5px;
            border-bottom: 1px solid #ddd;
            text-align: left;
        }
        #upcoming-tasks {
            background: rgba(255, 255, 255, 0.95);
            border: 2px dashed #b22234;
            padding: 10px;
            margin: 15px 0;
            border-radius: 5px;
            color: #001f3f;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        #upcoming-tasks:hover {
            background: #e7e7e7;
            border-color: #3c3b6e;
        }
        
        .hidden-list {
            display: none;
            color: #b22234;
            font-style: italic;
        }
        #objective {
            font-weight: bold;
            margin: 15px 0;
            color: #b22234;
            text-shadow: 1px 1px 0px rgba(255,255,255,0.5);
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
        
        /* Add pulsing effect for bad choices */
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

        @keyframes flashyTitle {
            0% { transform: scale(1); text-shadow: 0 0 10px #b22234; }
            25% { transform: scale(1.05); text-shadow: 0 0 20px #3c3b6e; }
            50% { transform: scale(1); text-shadow: 0 0 30px #b22234; }
            75% { transform: scale(1.05); text-shadow: 0 0 20px #3c3b6e; }
            100% { transform: scale(1); text-shadow: 0 0 10px #b22234; }
        }

        .welcome-title {
            animation: flashyTitle 2s infinite;
            font-size: 2.5em;
            margin-bottom: 20px;
            background: linear-gradient(45deg, #b22234, #3c3b6e);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 10px rgba(255,255,255,0.5);
        }

        .welcome-message {
            font-size: 1.2em;
            color: #3c3b6e;
            margin: 20px 0;
            padding: 15px;
            border: 2px solid #b22234;
            border-radius: 10px;
            background: rgba(255,255,255,0.9);
            box-shadow: 0 0 15px rgba(60,59,110,0.3);
        }

        /* Mobile Optimizations */
        @media (max-width: 768px) {
            body {
                font-size: 16px;
                -webkit-tap-highlight-color: transparent;
                overscroll-behavior: contain;
                position: relative;
                height: auto;
                min-height: 100vh;
                overflow-y: auto;
            }
            
            #game-container {
                max-width: 100%;
                margin: 10px;
                padding: 10px;
                touch-action: pan-y pinch-zoom;
                overflow-y: auto;
                -webkit-overflow-scrolling: touch;
            }
            
            #actions {
                grid-template-columns: repeat(2, 1fr);
                gap: 8px;
                margin-bottom: 20px;
            }
            
            button {
                padding: 15px;
                font-size: 1.1em;
                min-height: 60px;
                touch-action: manipulation;
                margin-bottom: 5px;
            }
            
            #modal {
                position: fixed;
                top: 50%;
                left: 50%;
                transform: translate(-50%, -50%);
                max-height: 90vh;
                overflow-y: auto;
                -webkit-overflow-scrolling: touch;
            }
            
            #modal-content {
                width: 90%;
                max-width: 350px;
                margin: 20px auto;
                padding: 15px;
                font-size: 1.1em;
                overflow-y: auto;
            }
            
            #game-log {
                height: 120px;
                font-size: 0.9em;
                -webkit-overflow-scrolling: touch;
            }
            
            .welcome-title {
                font-size: 2em;
            }
            
            /* Prevent text selection */
            * {
                -webkit-touch-callout: none;
                -webkit-user-select: none;
                user-select: none;
            }
            
            /* Better touch feedback */
            button:active {
                transform: scale(0.95);
                transition: transform 0.1s;
            }
            
            /* Landscape mode optimizations */
            @media (orientation: landscape) {
                #game-container {
                    display: grid;
                    grid-template-columns: 1fr 1fr;
                    gap: 10px;
                }
                
                #actions {
                    grid-template-columns: repeat(3, 1fr);
                }
                
                #game-log {
                    height: 200px;
                }
            }
        }
        
        /* Improved button visibility */
        button {
            text-shadow: 1px 1px 2px rgba(0,0,0,0.8);
            box-shadow: 0 2px 4px rgba(0,0,0,0.2);
        }
        
        /* Enhanced visual feedback */
        .tempting-choice {
            animation: temptPulse 1.5s infinite;
        }
        
        @keyframes temptPulse {
            0% { transform: scale(1); box-shadow: 0 0 15px rgba(255, 215, 0, 0.5); }
            50% { transform: scale(1.02); box-shadow: 0 0 25px rgba(255, 215, 0, 0.8); }
            100% { transform: scale(1); box-shadow: 0 0 15px rgba(255, 215, 0, 0.5); }
        }
        
        /* Better visibility for game stats */
        #stats {
            background: linear-gradient(45deg, #f7f7f7, #e7e7e7);
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        
        /* Improved modal readability */
        #modal-content {
            box-shadow: 0 4px 20px rgba(0,0,0,0.4);
            background: linear-gradient(45deg, #fff, #f7f7f7);
        }
    </style>
</head>
<body>
    <div id="game-container">
        <h1>BAD MAILMAN 🇺🇸</h1>
        
        <!-- Main Menu -->
        <div id="main-menu">
            <button onclick="showStartMenu()">Start Game</button>
        </div>

        <!-- Game Content -->
        <div id="game-content" style="display: none;">
            <div id="stats">
                <div class="stat-item">Health: <span id="health">100</span></div>
                <div class="stat-item">Cash: $<span id="cash">50</span></div>
                <div class="stat-item">Level: <span id="level">1</span>/3</div>
                <div class="stat-item">Time: <span id="timer">180</span>s</div>
            </div>

            <div id="objective">
                <h3>Current Mission:</h3>
                <p>Next Delivery: <span id="current-address"></span> 🇺🇸</p>
                <p>Deliveries: <span id="deliveries">0</span>/<span id="required-deliveries">3</span></p>
                <p>Tasks: <span id="tasks">0</span>/<span id="required-tasks">3</span></p>
            </div>

            <div id="upcoming-tasks">
                <h3>Delivery Information</h3>
                <p>Current Delivery: <span id="current-address"></span></p>
                <p>Time for delivery: <span id="task-timer">30</span>s</p>
                <p class="hidden-list" id="upcoming-list"></p>
                <p style="font-style: italic; color: #666;">Click to check delivery list</p>
            </div>

            <div id="actions">
                <button onclick="performAction('deliver')">Deliver Mail</button>
                <button onclick="performAction('water')">Drink Water</button>
                <button onclick="performAction('lunch')">Take Lunch</button>
                <button onclick="performAction('skip')">Skip Delivery</button>
                <button onclick="performAction('drive')">Drive Carefully</button>
                <button onclick="performAction('return')">Return to Office</button>
                <button onclick="performAction('dog')">Play with Dog</button>
                <button onclick="performAction('drink')">Take a Drink</button>
            </div>

            <div id="game-log"></div>
        </div>
    </div>

    <!-- Modal -->
    <div id="modal">
        <div id="modal-content">
            <p id="modal-text"></p>
            <div id="modal-options"></div>
            <div id="decision-timer">Time: <span id="decision-time">5</span>s</div>
        </div>
    </div>

    <script>
        // Constants
        const ADDRESSES = [
            // Irving Variations
            "42 Irving St", "42 Irving Dr", "42 Irving Ave", "42 Irving Ct",
            "44 Irving St", "44 Irving Dr", "44 Irving Ave", "44 Irving Ct",
            "46 Irving St", "46 Irving Dr", "46 Irving Ave", "46 Irving Ct",
            // Washington Variations
            "101 Washington St", "101 Washington Ave", "101 Washington Blvd", "101 Washington Dr",
            "103 Washington St", "103 Washington Ave", "103 Washington Blvd", "103 Washington Dr",
            // Oak Variations
            "220 Oak St", "220 Oak Ave", "220 Oak Ln", "220 Oak Circle",
            "222 Oak St", "222 Oak Ave", "222 Oak Ln", "222 Oak Circle",
            // Maple Variations
            "335 Maple Dr", "335 Maple St", "335 Maple Ave", "335 Maple Ct",
            "337 Maple Dr", "337 Maple St", "337 Maple Ave", "337 Maple Ct"
        ];

        const ORIGIN_STORY = [
            "You were once a dedicated mailman, loved by your community. Your route was perfect, your smile genuine.",
            "Then the economy crashed. Your pension vanished overnight. Medical bills piled up after your work injury.",
            "Your wife needed expensive surgery, but insurance denied the claim. She left, taking the kids and your hope.",
            "The drinking started small - just to numb the pain. The gambling seemed like a way out. The loan sharks came.",
            "Your supervisor denied your overtime, laughing about his new boat while you ate cold soup in your car.",
            "The local gangs saw your desperation. They offered quick cash for 'misplacing' certain packages.",
            "One 'lost' package became two, then ten. The money was good, and nobody seemed to care anymore.",
            "Your old partner reported some missing mail. Next day, he had a 'mysterious' accident. The message was clear.",
            "Now you're in too deep. Each delivery is a chance to either crawl back to dignity or embrace the corruption.",
            "The system broke you. Will you try to fix it, or burn it all down?"
        ];

        // Add more complex temptation scenarios
        const TEMPTATIONS = [
            {
                text: "A shady character offers you some 'premium powder' for quick energy...",
                options: [
                    { text: "Take cocaine (+Energy, +Cash)", effect: { health: -20, cash: 30, type: "bad" }, outcome: "The powder gives you a rush, but at what cost?" },
                    { text: "Deal some on your route (+Big Cash!)", effect: { health: -30, cash: 100, type: "bad" }, outcome: "You're a dealer now. The money's good, but your soul..." },
                    { text: "Stick to coffee", effect: { health: 5, cash: -5, type: "good" }, outcome: "You stay clean, but feel the grind." },
                    { text: "Report to police (+Karma)", effect: { health: 10, cash: 10, type: "good" }, outcome: "Doing the right thing feels good, but was it worth it?" }
                ]
            },
            {
                text: "You spot an illegal gambling den in your route...",
                options: [
                    { text: "Place a small bet (+Cash?)", effect: { health: -10, cash: 50, type: "bad" }, outcome: "Lucky streak! But the addiction grows..." },
                    { text: "Go all in! (+Big Cash?!)", effect: { health: -25, cash: 150, type: "bad" }, outcome: "JACKPOT! But at what cost to your sanity?" },
                    { text: "Become a bookie (+Steady Cash)", effect: { health: -20, cash: 80, type: "bad" }, outcome: "The house always wins, and now you're the house." },
                    { text: "Keep walking", effect: { health: 5, cash: 0, type: "good" }, outcome: "Your wallet's lighter but your conscience is clear." }
                ]
            },
            {
                text: "A suspicious package feels heavy with cash...",
                options: [
                    { text: "Steal it (+Cash)", effect: { health: -15, cash: 40, type: "bad" }, outcome: "Easy money, but your soul feels heavier." },
                    { text: "Sell info to thieves (+Big Cash)", effect: { health: -25, cash: 120, type: "bad" }, outcome: "You're part of a criminal network now." },
                    { text: "Deliver properly", effect: { health: 5, cash: 5, type: "good" }, outcome: "Integrity intact, but poverty continues." },
                    { text: "'Lose' it accidentally", effect: { health: -10, cash: 25, type: "bad" }, outcome: "Plausible deniability, but guilt gnaws at you." }
                ]
            },
            {
                text: "Your supervisor's drawer is open, revealing payroll...",
                options: [
                    { text: "Take a 'bonus' (+Cash)", effect: { health: -15, cash: 45, type: "bad" }, outcome: "The money's good, but paranoia sets in." },
                    { text: "Blackmail opportunity (+Regular Cash)", effect: { health: -20, cash: 70, type: "bad" }, outcome: "Power feels good, but you're becoming the villain." },
                    { text: "Close the drawer", effect: { health: 5, cash: 0, type: "good" }, outcome: "Honesty doesn't pay the bills, but you sleep better." },
                    { text: "Plant evidence (-Boss?)", effect: { health: -25, cash: 100, type: "bad" }, outcome: "Your boss is gone, but you're becoming a monster." }
                ]
            },
            {
                text: "A local gang offers protection and benefits...",
                options: [
                    { text: "Join them (+Power, +Cash)", effect: { health: -30, cash: 150, type: "bad" }, outcome: "You're protected now, but at what cost?" },
                    { text: "Become informant (+Both Sides)", effect: { health: -25, cash: 90, type: "bad" }, outcome: "Playing both sides is profitable but dangerous." },
                    { text: "Decline politely", effect: { health: -10, cash: -20, type: "good" }, outcome: "You stay clean, but they're watching you." },
                    { text: "Threaten to report them", effect: { health: -40, cash: -50, type: "good" }, outcome: "Brave but foolish. Watch your back." }
                ]
            }
        ];

        // Add more chaotic random events
        const CHAOS_EVENTS = [
            {
                text: "Your mail truck's brakes feel loose on a hill...",
                options: [
                    { text: "Crash into expensive cars (+Insurance Scam)", effect: { health: -30, cash: 200, type: "bad" }, outcome: "The 'accident' pays well, but your body aches..." },
                    { text: "Risk the hill anyway (+Thrill)", effect: { health: -20, cash: 0, type: "bad" }, outcome: "Pure adrenaline rush, but at what cost?" },
                    { text: "Report for maintenance (-Time)", effect: { health: 5, cash: -20, type: "good" }, outcome: "Safety first, but your wallet suffers." }
                ]
            },
            {
                text: "Police lights flash behind you. The trunk has 'special' packages...",
                options: [
                    { text: "Run for it! (+Excitement)", effect: { health: -40, cash: -50, type: "bad" }, outcome: "The chase was wild, but you're shaking..." },
                    { text: "Bribe the officer (+Corrupt)", effect: { health: -10, cash: -100, type: "bad" }, outcome: "Another soul joins the darkness." },
                    { text: "Face the music (-Freedom)", effect: { health: -50, cash: -200, type: "good" }, outcome: "Honesty hurts... a lot." }
                ]
            },
            {
                text: "Your truck breaks down in gang territory...",
                options: [
                    { text: "Join their operation (+Protection)", effect: { health: -20, cash: 150, type: "bad" }, outcome: "You're part of the family now..." },
                    { text: "Offer them mail routes (+Business)", effect: { health: -15, cash: 100, type: "bad" }, outcome: "A profitable partnership begins." },
                    { text: "Call for backup (-Pride)", effect: { health: -10, cash: -30, type: "good" }, outcome: "The wait was long, but you stayed clean." }
                ]
            },
            {
                text: "You find undelivered mail from months ago in the warehouse...",
                options: [
                    { text: "Sell personal info (+Quick Cash)", effect: { health: -25, cash: 180, type: "bad" }, outcome: "Identity theft pays well..." },
                    { text: "Burn it all (+Cover-up)", effect: { health: -15, cash: 50, type: "bad" }, outcome: "The evidence goes up in smoke." },
                    { text: "Report it (-Career)", effect: { health: -20, cash: -40, type: "good" }, outcome: "Your honesty is not appreciated." }
                ]
            }
        ];

        // Game state
        let gameState = {
            health: 100,
            cash: 50,
            level: 1,
            timer: 180,
            deliveries: 0,
            tasks: 0,
            gameInterval: null,
            taskTimer: 30,
            taskInterval: null,
            decisionTimer: null,
            currentAddress: "",
            upcomingDeliveries: [],
            storyIndex: 0,
            badChoices: 0,
            goodChoices: 0,
            decisionTime: 8,
            showOriginStory: false,
            lastDogInteraction: 0,
            dogInteractions: 0,
            usedAddresses: new Set(),
            currentLevelDeliveries: [],
            completedTaskTypes: new Set()
        };

        // Level configurations with varying challenges
        const LEVELS = {
            1: {
                deliveries: 3,
                tasks: 3,
                timeBonus: 60,
                actions: {
                    deliver: { reward: 10, health: -5, type: "delivery" },
                    water: { reward: 5, health: 10, type: "task" },
                    lunch: { reward: -5, health: 15, type: "task" },
                    skip: { reward: -10, health: 0, type: "none" },
                    check: { reward: -10, health: -5, type: "none" },
                    return: { reward: -20, health: -10, type: "none" },
                    rest: { reward: 0, health: 20, type: "task" },
                    organize: { reward: 5, health: 5, type: "task" },
                    exercise: { reward: -5, health: 15, type: "task" }
                }
            },
            2: {
                deliveries: 4,
                tasks: 4,
                timeBonus: 90,
                actions: {
                    deliver: { reward: 15, health: -8, type: "delivery" },
                    steal: { reward: 30, health: -20, type: "bad" },
                    water: { reward: 8, health: 12, type: "task" },
                    drugs: { reward: -20, health: 25, type: "bad" },
                    gamble: { reward: -30, health: -15, type: "bad" },
                    check: { reward: -15, health: -8, type: "none" },
                    lunch: { reward: -8, health: 18, type: "task" },
                    bribe: { reward: 25, health: -15, type: "bad" },
                    exercise: { reward: -8, health: 20, type: "task" }
                }
            },
            3: {
                deliveries: 5,
                tasks: 5,
                timeBonus: 120,
                actions: {
                    deliver: { reward: 20, health: -12, type: "delivery" },
                    scam: { reward: 50, health: -25, type: "bad" },
                    water: { reward: 12, health: 15, type: "task" },
                    dealer: { reward: 100, health: -40, type: "bad" },
                    extort: { reward: 40, health: -30, type: "bad" },
                    check: { reward: -20, health: -10, type: "none" },
                    lunch: { reward: -10, health: 20, type: "task" },
                    crime: { reward: 150, health: -50, type: "bad" },
                    exercise: { reward: -10, health: 25, type: "task" }
                }
            }
        };

        // Game functions
        function showStartMenu() {
            const mainMenu = document.getElementById("main-menu");
            mainMenu.innerHTML = `
                <h1 class="welcome-title">BAD MAILMAN 🇺🇸</h1>
                <div class="welcome-message">
                    <p>⏱️ 3 INTENSE LEVELS - EACH TIMED!</p>
                    <p>🎯 Complete deliveries, maintain health, earn cash</p>
                    <p>⚠️ But beware... temptation lurks around every corner...</p>
                </div>
            `;
            
            showTimedChoice(
                "Ready to begin your descent?",
                [
                    { text: "Quick Start ⚡", effect: { type: "start" }, outcome: "Starting game..." },
                    { text: "Origin Story 📖", effect: { type: "origin" }, outcome: "Loading origin story..." }
                ],
                8,
                (choice) => {
                    if (choice.effect.type === "origin") {
                        gameState.showOriginStory = true;
                        startOriginStory();
                    } else {
                        gameState.showOriginStory = false;
                        startGame();
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
            resetGameState();
            document.getElementById("main-menu").style.display = "none";
            document.getElementById("game-content").style.display = "block";
            initializeDeliveries();
            
            // Show initial delivery list with 10-second timer
            showTimedChoice(
                `🚨 MEMORIZE YOUR DELIVERIES! 🚨\n\n${gameState.upcomingDeliveries.join(" → ")}\n\nYou have 10 seconds to memorize these addresses.\nLevel 1: Address list visible\nLevel 2-3: Must pay to check list\n\nThe criminal path is always easier...`,
                [{ text: "I'm Ready!", effect: { type: "start" }, outcome: "Starting game..." }],
                10,
                () => {
                    hideModal();
                    startTimers();
                    updateUI();
                    updateButtons();
                    addToLog("Game started! The honest path awaits... for now.");
                    updateDeliveryListVisibility();
                }
            );
        }

        function resetGameState() {
            gameState = {
                health: 100,
                cash: 50,
                level: 1,
                timer: 180,
                deliveries: 0,
                tasks: 0,
                gameInterval: null,
                taskTimer: 30,
                taskInterval: null,
                decisionTimer: null,
                currentAddress: "",
                upcomingDeliveries: [],
                storyIndex: 0,
                badChoices: 0,
                goodChoices: 0,
                decisionTime: 8,
                showOriginStory: false,
                lastDogInteraction: 0,
                dogInteractions: 0,
                usedAddresses: new Set(),
                currentLevelDeliveries: [],
                completedTaskTypes: new Set()
            };
        }

        function initializeDeliveries() {
            // Reset used addresses when starting a new level
            gameState.usedAddresses = new Set();
            gameState.currentLevelDeliveries = [];
            
            // Get fresh addresses for this level
            let availableAddresses = ADDRESSES.filter(addr => !gameState.usedAddresses.has(addr));
            
            // If we're running low on addresses, reset the used addresses
            if (availableAddresses.length < 5) {
                gameState.usedAddresses.clear();
                availableAddresses = [...ADDRESSES];
            }

            // Select 5 random addresses for this level
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
            // Clear any existing timers first
            if (gameState.gameInterval) clearInterval(gameState.gameInterval);
            if (gameState.taskInterval) clearInterval(gameState.taskInterval);
            
            // Main game timer
            gameState.gameInterval = setInterval(() => {
                gameState.timer--;
                if (gameState.timer <= 0) {
                    endGame("⏰ Time's up! Your shift has ended.");
                }
                if (gameState.health <= 0) {
                    endGame("💔 You ran out of health! The job was too much to handle.");
                }
                updateUI();
            }, 1000);

            // Delivery task timer
            gameState.taskInterval = setInterval(() => {
                gameState.taskTimer--;
                if (gameState.taskTimer <= 0) {
                    failDelivery();
                }
                updateUI();
            }, 1000);
        }

        function performAction(actionType) {
            if (actionType === 'deliver') {
                showDeliveryChallenge();
                return;
            }

            if (actionType === 'dog') {
                const currentTime = Date.now();
                const timeSinceLastDog = currentTime - gameState.lastDogInteraction;
                
                if (timeSinceLastDog < 10000 && gameState.lastDogInteraction !== 0) {
                    addToLog("The dog needs a break! Try something else for now.");
                    return;
                }

                // If this task type has already been completed, offer a temptation instead
                if (gameState.completedTaskTypes.has('dog')) {
                    showTimedChoice(
                        "You've already bonded with this dog today...",
                        [
                            { text: "Train for crime (+Cash)", 
                              effect: { health: -20, cash: 50, type: "bad" },
                              outcome: "The dog becomes your partner in crime...",
                              tempting: true },
                            { text: "Just play (+Health)", 
                              effect: { health: 5, cash: 0, type: "none" },
                              outcome: "A simple moment of joy, but no task completed." }
                        ],
                        gameState.decisionTime,
                        handleDogChoice
                    );
                    return;
                }

                // First time completing this task type
                gameState.completedTaskTypes.add('dog');
                gameState.lastDogInteraction = currentTime;
                gameState.dogInteractions++;

                // Different scenarios based on number of interactions
                if (gameState.dogInteractions === 1) {
                    // First interaction is always positive
                    const healthGain = 10 + (Math.random() * 5);
                    gameState.health += healthGain;
                    gameState.tasks++;
                    addToLog(`Played with a friendly dog: +${Math.floor(healthGain)} Health, +1 Task. Your spirit lifts!`);
                } else if (gameState.dogInteractions === 2) {
                    // Second interaction has a choice
                    showTimedChoice(
                        "The dog has a package in its mouth...",
                        [
                            { text: "Play fetch normally (+Health)", 
                              effect: { health: 10, cash: 0, type: "good" }, 
                              outcome: "A wholesome game of fetch brightens your day!" },
                            { text: "Train dog to steal mail (+Cash)", 
                              effect: { health: -10, cash: 30, type: "bad" }, 
                              outcome: "The dog learns to bring you packages... this could be useful.",
                              tempting: true }
                        ],
                        gameState.decisionTime,
                        (choice) => {
                            handleDogChoice(choice);
                            if (choice.effect.type === "good") {
                                gameState.tasks++;
                            }
                        }
                    );
                } else if (gameState.dogInteractions === 3) {
                    // Third interaction is a temptation
                    showTimedChoice(
                        "The dog seems very well-trained now...",
                        [
                            { text: "Keep playing normally (+Health)", 
                              effect: { health: 8, cash: 0, type: "good" }, 
                              outcome: "Simple pleasures are sometimes the best." },
                            { text: "Use dog as lookout (+Task, +Cash)", 
                              effect: { health: -15, cash: 40, type: "bad" }, 
                              outcome: "The dog will warn you of approaching supervisors...",
                              tempting: true },
                            { text: "Train dog pack (+Big Cash)", 
                              effect: { health: -25, cash: 100, type: "bad" }, 
                              outcome: "You now have a network of mail-stealing dogs!",
                              tempting: true }
                        ],
                        gameState.decisionTime,
                        (choice) => {
                            handleDogChoice(choice);
                            if (choice.effect.type === "good") {
                                gameState.tasks++;
                            }
                        }
                    );
                } else {
                    // Subsequent interactions are random events
                    const events = [
                        {
                            text: "The dog brings you a suspicious package...",
                            options: [
                                { text: "Just play fetch (+Health)", 
                                  effect: { health: 5, cash: 0, type: "good" }, 
                                  outcome: "You ignore the temptation." },
                                { text: "Check package contents (+Cash?)", 
                                  effect: { health: -10, cash: 25, type: "bad" }, 
                                  outcome: "Easy money, but at what cost?",
                                  tempting: true }
                            ]
                        },
                        {
                            text: "The dog seems to know where valuable packages are...",
                            options: [
                                { text: "Regular playtime (+Health)", 
                                  effect: { health: 5, cash: 0, type: "good" }, 
                                  outcome: "A moment of peace in a corrupt world." },
                                { text: "Follow the dog (+Cash)", 
                                  effect: { health: -15, cash: 35, type: "bad" }, 
                                  outcome: "The dog's nose for valuables pays off...",
                                  tempting: true }
                            ]
                        }
                    ];
                    
                    const event = events[Math.floor(Math.random() * events.length)];
                    showTimedChoice(
                        event.text,
                        shuffleArray(event.options),
                        gameState.decisionTime,
                        (choice) => {
                            handleDogChoice(choice);
                            if (choice.effect.type === "good" && Math.random() < 0.3) {
                                gameState.tasks++;
                                addToLog("The wholesome interaction counts as a task!");
                            }
                        }
                    );
                }
                
                checkProgress();
                updateUI();
                return;
            }

            if (actionType === 'drink') {
                const healthLoss = 15 + (gameState.level * 5);
                const cashGain = 20 + (gameState.level * 10);
                gameState.health -= healthLoss;
                gameState.cash += cashGain;
                addToLog(`Had a drink at the bar: -${healthLoss} Health, +$${cashGain}. The temptation grows...`);
                
                // Chance for a drinking-related event
                if (Math.random() < 0.4) {
                    showTemptation();
                }
                return;
            }

            if (actionType === 'skip') {
                // Harsh penalties for skipping
                const skipPenalty = 15 + (gameState.level * 5);
                const healthPenalty = 10 + (gameState.level * 3);
                gameState.health -= healthPenalty;
                gameState.cash -= skipPenalty;
                addToLog(`Skipped delivery: -$${skipPenalty}, -${healthPenalty} Health. Your reputation suffers...`);
                completeDelivery();
                
                // Random chance of supervisor confrontation
                if (Math.random() < 0.4) {
                    showTimedChoice(
                        "Your supervisor noticed the skipped delivery...",
                        [
                            { text: "Make excuses (-Health)", effect: { health: -10, cash: 0, type: "none" }, outcome: "He doesn't believe you..." },
                            { text: "Apologize (-Cash)", effect: { health: 0, cash: -20, type: "none" }, outcome: "It'll come out of your paycheck..." },
                            { text: "Threaten him (+Cash, -Health)", effect: { health: -20, cash: 30, type: "bad" }, outcome: "He backs down, but at what cost?", tempting: true }
                        ],
                        gameState.decisionTime,
                        handleTemptationChoice
                    );
                }
                return;
            }

            if (actionType === 'return') {
                // Early return consequences
                const totalDeliveries = LEVELS[gameState.level].deliveries;
                const totalTasks = LEVELS[gameState.level].tasks;
                const completionPercent = ((gameState.deliveries / totalDeliveries) + (gameState.tasks / totalTasks)) * 50;
                
                showTimedChoice(
                    `Return to office early? (${Math.floor(completionPercent)}% complete)\nThis will end your shift immediately!`,
                    [
                        { text: "Return anyway", effect: { type: "return" }, outcome: "Shift ended early..." },
                        { text: "Keep working", effect: { type: "continue" }, outcome: "Back to work..." }
                    ],
                    gameState.decisionTime,
                    (choice) => {
                        if (choice.effect.type === "return") {
                            // Apply harsh penalties and end game
                            gameState.cash = Math.floor(gameState.cash * (completionPercent / 100));
                            gameState.health -= 30;
                            endGame("Returned to office early. Management is disappointed...");
                        }
                        hideModal();
                    }
                );
                return;
            }

            // Increase temptation chance based on low cash or health
            let temptationChance = 0.3;
            if (gameState.cash < 30) temptationChance += 0.2;
            if (gameState.health < 50) temptationChance += 0.2;

            if (Math.random() < temptationChance) {
                if (Math.random() < 0.5) {
                    showTemptation();
                } else {
                    addRandomEvent();
                }
                return;
            }

            const levelConfig = LEVELS[gameState.level];
            const action = levelConfig.actions[actionType];

            if (!action) {
                console.error(`Unknown action: ${actionType}`);
                return;
            }

            gameState.health += action.health;
            gameState.cash += action.reward;

            if (action.type === "delivery") {
                completeDelivery();
            } else if (action.type === "task") {
                gameState.tasks++;
            }

            addToLog(`${actionType}: Health ${action.health > 0 ? "+" : ""}${action.health}, Cash ${action.reward > 0 ? "+" : ""}$${action.reward}`);
            checkProgress();
            updateUI();
        }

        function showDeliveryChallenge() {
            const correctAddress = gameState.currentAddress;
            const [number, ...streetParts] = correctAddress.split(" ");
            const streetBase = streetParts.slice(0, -1).join(" ");
            const streetType = streetParts[streetParts.length - 1];
            
            // Generate extremely similar addresses
            const similarAddresses = [
                `${number} ${streetBase} ${getRandomStreetType(streetType)}`,
                `${parseInt(number) + 2} ${streetBase} ${streetType}`,
                `${number} ${getSlightlyModifiedStreet(streetBase)} ${streetType}`
            ];
            
            // Base criminal rewards on level and player state
            const baseReward = 50 + (gameState.level * 30);
            const desperation = (100 - gameState.health) + (50 - gameState.cash);
            const bonusReward = Math.floor(desperation * 1.2);

            // Create options array with correct address and criminal choices
            const options = [
                { text: correctAddress, 
                  effect: { health: 5, cash: 15, type: "delivery" }, 
                  outcome: "Perfect delivery! But the temptation grows..." }
            ];

            // Add wrong addresses
            similarAddresses.forEach(addr => {
                options.push({
                    text: addr,
                    effect: { health: -10, cash: -10, type: "none" },
                    outcome: "Wrong address! The honest path feels harder..."
                });
            });

            // Add criminal options based on level
            const criminalOptions = [
                { text: `💰 "Lose" the package (+$${baseReward})`,
                  effect: { health: -15, cash: baseReward, type: "bad" },
                  outcome: "Easy money... The first step into darkness.",
                  tempting: true }
                ];

            // Add more serious criminal options for higher levels
            if (gameState.level >= 2) {
                criminalOptions.push(
                    { text: `💎 Sell package contents (+$${baseReward + bonusReward})`,
                      effect: { health: -20, cash: baseReward + bonusReward, type: "bad" },
                      outcome: "The money feels good... Maybe being bad isn't so wrong?",
                      tempting: true }
                );
            }
            if (gameState.level === 3) {
                criminalOptions.push(
                    { text: `👑 Start a mail theft empire (+$${baseReward * 3})`,
                      effect: { health: -40, cash: baseReward * 3, type: "bad" },
                      outcome: "Power. Control. Money. Everything you deserve...",
                      tempting: true }
                );
            }

            // Add criminal options to the choices
            options.push(...criminalOptions);

            // Show choices with simplified text
            showTimedChoice(
                `📬 MAKE YOUR DELIVERY\n
✅ Correct: +Health, +$15
❌ Wrong: -Health, -$10
💰 Criminal: More Cash, Less Soul\n
Choose...`,
                shuffleArray(options),
                gameState.decisionTime,
                handleDeliveryChoice
            );
        }

        function getRandomStreetType(current) {
            const types = ['St', 'Ave', 'Dr', 'Ct', 'Ln', 'Circle', 'Blvd', 'Place', 'Road'];
            let available = types.filter(t => t !== current);
            return available[Math.floor(Math.random() * available.length)];
        }

        function getSlightlyModifiedStreet(street) {
            // Slightly modify street name to be very similar
            const modifications = {
                'Irving': ['Irwing', 'Erving', 'Irvine'],
                'Washington': ['Washingten', 'Washinton', 'Washenton'],
                'Oak': ['Oake', 'Oke', 'Oaks'],
                'Maple': ['Mapel', 'Maples', 'Mapple']
            };
            
            for (let [original, variants] of Object.entries(modifications)) {
                if (street.includes(original)) {
                    return street.replace(original, variants[Math.floor(Math.random() * variants.length)]);
                }
            }
            return street;
        }

        function showTemptation() {
            const temptation = TEMPTATIONS[Math.floor(Math.random() * TEMPTATIONS.length)];
            showTimedChoice(
                temptation.text,
                shuffleArray(temptation.options),
                gameState.decisionTime,
                handleTemptationChoice
            );
        }

        function showTimedChoice(text, options, time, callback) {
            // Clear any existing timer
            if (gameState.decisionTimer) {
                clearInterval(gameState.decisionTimer);
                gameState.decisionTimer = null;
            }

            const modal = document.getElementById("modal");
            const modalText = document.getElementById("modal-text");
            const modalOptions = document.getElementById("modal-options");
            const timerDisplay = document.getElementById("decision-time");

            modalText.textContent = text;
            modalOptions.innerHTML = "";
            
            options.forEach(option => {
                const button = document.createElement("button");
                button.className = "modal-option";
                if (option.tempting) {
                    button.classList.add("tempting-choice");
                }
                button.textContent = option.text;
                button.onclick = () => {
                    if (gameState.decisionTimer) {
                        clearInterval(gameState.decisionTimer);
                        gameState.decisionTimer = null;
                    }
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
                    if (gameState.decisionTimer) {
                        clearInterval(gameState.decisionTimer);
                        gameState.decisionTimer = null;
                    }
                    handleTimeout();
                }
            }, 1000);
        }

        function handleTimeout() {
            clearInterval(gameState.decisionTimer);
            gameState.health -= 15;
            gameState.cash -= 10;
            
            if (gameState.health === 100 && gameState.cash === 50) {
                // We're at the start menu
                gameState.showOriginStory = true;
                startOriginStory();
            } else {
                addToLog("You hesitated! Lost health and cash.");
                hideModal();
                updateUI();
            }
        }

        function handleDeliveryChoice(choice) {
            vibrateDevice(100); // Vibrate on delivery choice
            clearInterval(gameState.decisionTimer);
            gameState.health += choice.effect.health;
            gameState.cash += choice.effect.cash;
            
            if (choice.effect.type === "delivery") {
                completeDelivery();
            }
            if (choice.effect.type === "bad") {
                gameState.badChoices++;
            }

            addToLog(choice.outcome);
            hideModal();
            updateUI();
        }

        function handleTemptationChoice(choice) {
            clearInterval(gameState.decisionTimer);
            gameState.health += choice.effect.health;
            gameState.cash += choice.effect.cash;
            
            if (choice.effect.type === "bad") {
                gameState.badChoices++;
            } else {
                gameState.goodChoices++;
            }

            addToLog(choice.outcome);
            hideModal();
            updateUI();
        }

        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        function completeDelivery() {
            vibrateDevice(200); // Longer vibration for level up
            gameState.deliveries++;
            gameState.taskTimer = 30;
            gameState.currentLevelDeliveries.shift();
            
            // If we still have deliveries in the current level
            if (gameState.currentLevelDeliveries.length > 0) {
                gameState.currentAddress = gameState.currentLevelDeliveries[0];
            }
            
            // Debug log for delivery completion
            console.log(`Delivery completed! Total: ${gameState.deliveries}`);
            
            updateUpcomingDeliveries();
            checkProgress(); // Check progress after completing delivery
            saveGameState();
        }

        function failDelivery() {
            gameState.health -= 15;
            gameState.cash -= 10;
            addToLog("Delivery failed! Lost 15 Health and $10");
            completeDelivery();
        }

        function checkProgress() {
            const levelConfig = LEVELS[gameState.level];
            
            // Debug log to check progress
            console.log(`Checking progress - Level: ${gameState.level}, Deliveries: ${gameState.deliveries}/${levelConfig.deliveries}, Tasks: ${gameState.tasks}/${levelConfig.tasks}`);
            
            if (gameState.deliveries >= levelConfig.deliveries && 
                gameState.tasks >= levelConfig.tasks) {
                
                // Debug log for level completion
                console.log(`Level ${gameState.level} completed!`);
                
                if (gameState.level < 3) {
                    // Clear existing timers before level up
                    clearInterval(gameState.gameInterval);
                    clearInterval(gameState.taskInterval);
                    
                    // Initialize new deliveries before level up
                    initializeDeliveries();
                    levelUp();
                } else {
                    // Successfully completed all levels
                    endGame("🎉 Congratulations! You've completed all levels! Your journey from mailman to legend is complete!");
                }
            }
        }

        function levelUp() {
            vibrateDevice(200); // Longer vibration for level up
            gameState.level++;
            gameState.deliveries = 0;
            gameState.tasks = 0;
            gameState.timer += LEVELS[gameState.level].timeBonus;
            
            // Debug log for level up
            console.log(`Leveling up to ${gameState.level}!`);
            
            // Show new delivery list for memorization before starting next level
            showTimedChoice(
                `🎉 LEVEL ${gameState.level} UNLOCKED! 🎉\n\n🚨 MEMORIZE THESE ADDRESSES! 🚨\n\n${gameState.currentLevelDeliveries.join(" → ")}\n\nYou have 10 seconds to memorize.\nThe criminal path grows stronger...\n\nTime Bonus: +${LEVELS[gameState.level].timeBonus}s\nDeliveries Required: ${LEVELS[gameState.level].deliveries}\nTasks Required: ${LEVELS[gameState.level].tasks}`,
                [{ text: "I'm Ready!", effect: { type: "start" }, outcome: "Starting next level..." }],
                10,
                () => {
                    hideModal();
                    // Restart timers for new level
                    startTimers();
                    updateButtons();
                    updateDeliveryListVisibility();
                    addToLog(`🌟 Advanced to Level ${gameState.level}! The temptations grow stronger...`);
                    updateUI();
                    saveGameState();
                    gameState.completedTaskTypes.clear();  // Reset completed tasks for new level
                }
            );
        }

        function endGame(message) {
            clearInterval(gameState.gameInterval);
            clearInterval(gameState.taskInterval);
            clearInterval(gameState.decisionTimer);
            
            const finalScore = Math.floor(gameState.cash + (gameState.health * 2));
            const badnessRating = Math.floor((gameState.badChoices / (gameState.badChoices + gameState.goodChoices)) * 100) || 0;
            
            let endingMessage = "";
            if (badnessRating > 75) {
                endingMessage = "You've become a true villain. The mail system was just your first step into darkness.";
            } else if (badnessRating > 50) {
                endingMessage = "You walked the line between good and evil, taking what you needed to survive.";
            } else {
                endingMessage = "Against all odds, you maintained your integrity. Your will was stronger than temptation.";
            }
            
            showModal(
                `${message}\n\n` +
                `📊 FINAL RESULTS\n` +
                `Level: ${gameState.level}/3\n` +
                `Score: ${finalScore}\n` +
                `Health: ${Math.floor(gameState.health)}\n` +
                `Cash: $${gameState.cash}\n\n` +
                `📬 STATISTICS\n` +
                `Deliveries: ${gameState.deliveries}\n` +
                `Tasks: ${gameState.tasks}\n` +
                `Good/Bad: ${gameState.goodChoices}/${gameState.badChoices}\n\n` +
                `${endingMessage}`,
                [{ text: "Return to Main Menu", action: () => {
                    hideModal();
                    document.getElementById("game-content").style.display = "none";
                    document.getElementById("main-menu").style.display = "block";
                    showStartMenu();
                }}]
            );
        }

        function showModal(message, options) {
            const modal = document.getElementById("modal");
            const modalText = document.getElementById("modal-text");
            const modalOptions = document.getElementById("modal-options");

            modalText.textContent = message;
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
            document.getElementById("timer").textContent = gameState.timer;
            document.getElementById("deliveries").textContent = gameState.deliveries;
            document.getElementById("tasks").textContent = gameState.tasks;
            document.getElementById("current-address").textContent = gameState.currentAddress;
            document.getElementById("task-timer").textContent = gameState.taskTimer;
            document.getElementById("required-deliveries").textContent = LEVELS[gameState.level].deliveries;
            document.getElementById("required-tasks").textContent = LEVELS[gameState.level].tasks;
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

        function addRandomEvent() {
            const events = [...CHAOS_EVENTS, ...TEMPTATIONS];
            const event = events[Math.floor(Math.random() * events.length)];
            showTimedChoice(
                event.text,
                shuffleArray(event.options),
                gameState.decisionTime,
                handleTemptationChoice
            );
        }

        function checkTaskList() {
            const penalty = 10 + (gameState.level * 5);
            const healthLoss = 5 + (gameState.level * 2);
            
            gameState.cash -= penalty;
            gameState.health -= healthLoss;
            
            // Show delivery list in a modal instead of in-place
            showModal(
                `📝 DELIVERY LIST (MEMORIZE QUICKLY!):\n\n${gameState.currentLevelDeliveries.join(" → ")}\n\nPenalty paid: -$${penalty}, -${healthLoss} Health`,
                [{ text: "I've Memorized It", action: () => {
                    hideModal();
                    addToLog(`Checked address book: -$${penalty}, -${healthLoss} Health. The honest path feels harder...`);
                    updateUI();
                    
                    // Increase temptation after checking
                    if (Math.random() < 0.4) {
                        setTimeout(() => {
                            showTemptation();
                        }, 500);
                    }
                }}]
            );
        }

        function updateDeliveryListVisibility() {
            const upcomingTasks = document.getElementById("upcoming-tasks");
            const upcomingList = document.getElementById("upcoming-list");
            
            // Always hide the list initially
            upcomingList.style.display = "none";
            
            if (gameState.level === 1) {
                // Level 1: Allow checking the list without penalty
                upcomingTasks.onclick = () => {
                    showModal(
                        `📝 Current Delivery List:\n\n${gameState.currentLevelDeliveries.join(" → ")}`,
                        [{ text: "Got it!", action: hideModal }]
                    );
                };
                upcomingTasks.style.cursor = "pointer";
                upcomingTasks.title = "Click to view delivery list (Level 1: Free to check)";
            } else {
                // Level 2-3: Show with penalty
                upcomingTasks.onclick = () => {
                    const penalty = 10 + (gameState.level * 5);
                    const healthLoss = 5 + (gameState.level * 2);
                    
                    showTimedChoice(
                        `⚠️ Checking the delivery list will cost:\n-$${penalty}\n-${healthLoss} Health\n\nAre you sure?`,
                        [
                            { text: "Yes, show me (-$$$)", 
                              effect: { type: "show", health: -healthLoss, cash: -penalty }, 
                              outcome: "Checking list..." },
                            { text: "No, I'll try to remember", 
                              effect: { type: "cancel" }, 
                              outcome: "Cancelled" }
                        ],
                        gameState.decisionTime,
                        (choice) => {
                            if (choice.effect.type === "show") {
                                gameState.health += choice.effect.health;
                                gameState.cash += choice.effect.cash;
                                showModal(
                                    `📝 DELIVERY LIST:\n\n${gameState.currentLevelDeliveries.join(" → ")}\n\nPenalty paid: -$${penalty}, -${healthLoss} Health`,
                                    [{ text: "I've Memorized It", action: hideModal }]
                                );
                                addToLog(`Checked address list: -$${penalty}, -${healthLoss} Health`);
                                updateUI();
                                
                                // Chance for temptation after checking
                                if (Math.random() < 0.4) {
                                    setTimeout(showTemptation, 500);
                                }
                            }
                            hideModal();
                        }
                    );
                };
                upcomingTasks.style.cursor = "pointer";
                upcomingTasks.title = "Click to view delivery list (Costs money and health)";
            }
        }

        function handleDogChoice(choice) {
            gameState.health += choice.effect.health;
            gameState.cash += choice.effect.cash;
            
            if (choice.effect.type === "bad") {
                gameState.badChoices++;
            } else {
                gameState.goodChoices++;
            }
            
            addToLog(choice.outcome);
            hideModal();
            updateUI();
        }

        // Dynamic button configurations for different levels
        const LEVEL_BUTTONS = {
            1: [
                { action: 'deliver', text: 'Deliver Mail 🇺🇸' },
                { action: 'water', text: 'Drink Water (+Health)' },
                { action: 'lunch', text: 'Take Lunch (+Health)' },
                { action: 'skip', text: 'Skip Delivery' },
                { action: 'dog', text: 'Play with Dog (+Task)' },
                { action: 'return', text: 'Return to Office' },
                { action: 'rest', text: 'Take Break (+Health)' },
                { action: 'organize', text: 'Organize Bag (+Task)' },
                { action: 'exercise', text: 'Quick Exercise (+Health)' }
            ],
            2: [
                { action: 'deliver', text: 'Deliver Mail 🇺🇸' },
                { action: 'steal', text: 'Steal Package (+Cash)' },
                { action: 'water', text: 'Drink Water (+Health)' },
                { action: 'dog', text: 'Play with Dog (+Task)' },
                { action: 'drink', text: 'Visit Bar (+Cash)' },
                { action: 'skip', text: 'Skip Delivery' },
                { action: 'lunch', text: 'Take Lunch (+Health)' },
                { action: 'bribe', text: 'Bribe Customer' },
                { action: 'exercise', text: 'Quick Exercise (+Health)' }
            ],
            3: [
                { action: 'deliver', text: 'Deliver Mail 🇺🇸' },
                { action: 'scam', text: 'Run Mail Scam (+Big Cash)' },
                { action: 'water', text: 'Drink Water (+Health)' },
                { action: 'dog', text: 'Play with Dog (+Task)' },
                { action: 'drink', text: 'Visit Bar (+Cash)' },
                { action: 'skip', text: 'Skip Delivery' },
                { action: 'lunch', text: 'Take Lunch (+Health)' },
                { action: 'crime', text: 'Join Crime Ring' },
                { action: 'exercise', text: 'Quick Exercise (+Health)' }
            ]
        };

        function updateButtons() {
            const actionsDiv = document.getElementById("actions");
            actionsDiv.innerHTML = "";
            
            // Get all buttons for the current level
            const buttons = shuffleArray([...LEVEL_BUTTONS[gameState.level]]);
            
            // Ensure at least 9 buttons are shown (3x3 grid)
            while (buttons.length < 9) {
                buttons.push(buttons[Math.floor(Math.random() * buttons.length)]);
            }
            
            buttons.forEach(button => {
                const btn = document.createElement("button");
                btn.textContent = button.text;
                if (button.action === 'check') {
                    btn.onclick = checkTaskList;
                } else {
                    btn.onclick = () => performAction(button.action);
                }
                // Add American flag-themed hover effect
                btn.onmouseover = () => {
                    btn.style.background = '#3c3b6e';
                    btn.style.borderColor = '#b22234';
                };
                btn.onmouseout = () => {
                    btn.style.background = '#b22234';
                    btn.style.borderColor = '#b22234';
                };
                actionsDiv.appendChild(btn);
            });
        }

        // Add touch event handling
        document.addEventListener('touchstart', function(e) {
            if (e.touches.length > 1) {
                e.preventDefault(); // Prevent pinch zoom
            }
        }, { passive: false });
        
        // Prevent double-tap zoom
        document.addEventListener('dblclick', function(e) {
            e.preventDefault();
        });
        
        // Handle orientation changes
        window.addEventListener('orientationchange', function() {
            setTimeout(updateUI, 100); // Update UI after orientation change
        });
        
        // Add vibration feedback for important events
        function vibrateDevice(duration) {
            if ('vibrate' in navigator) {
                navigator.vibrate(duration);
            }
        }
        
        // Add audio feedback (optional, based on user preference)
        const gameAudio = {
            delivery: new Audio('data:audio/wav;base64,UklGRl9vT19...'), // Base64 encoded short sound
            levelUp: new Audio('data:audio/wav;base64,UklGRl9vT19...'),
            temptation: new Audio('data:audio/wav;base64,UklGRl9vT19...')
        };
        
        // Handle audio muting
        let audioEnabled = false;
        function toggleAudio() {
            audioEnabled = !audioEnabled;
            // Update audio button UI
        }
        
        // Add save game progress
        function saveGameState() {
            try {
                localStorage.setItem('badMailmanState', JSON.stringify(gameState));
            } catch (e) {
                console.error('Failed to save game state:', e);
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
                console.error('Failed to load game state:', e);
            }
        }
        
        // Add auto-save on important events
        function completeDelivery() {
            // ... existing code ...
            saveGameState();
        }
        
        function levelUp() {
            // ... existing code ...
            saveGameState();
        }
        
        // Add pull-to-refresh prevention
        document.body.style.overscrollBehavior = 'none';
        
        // Initialize mobile-specific features
        window.onload = function() {
            // Remove the fixed positioning that was preventing scrolling
            document.body.style.overflow = 'auto';
            document.body.style.position = 'relative';
            
            // Load saved game if exists
            loadGameState();
            
            // Start game
            showStartMenu();
        };
    </script>
</body>
</html> 
