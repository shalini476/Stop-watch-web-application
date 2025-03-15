<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modern Stopwatch</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: "Arial", sans-serif;
        }

        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: linear-gradient(135deg, #1e1e2f, #3a3a6e);
            color: white;
        }

        .container {
            background: rgba(255, 255, 255, 0.1);
            padding: 30px;
            border-radius: 15px;
            backdrop-filter: blur(10px);
            box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.3);
            text-align: center;
            width: 350px;
        }

        h1 {
            font-size: 28px;
            margin-bottom: 15px;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .display {
            font-size: 45px;
            font-weight: bold;
            padding: 15px 0;
            border-radius: 10px;
            background: rgba(255, 255, 255, 0.2);
            box-shadow: inset 0px 3px 10px rgba(0, 0, 0, 0.2);
            margin-bottom: 15px;
        }

        .buttons {
            display: flex;
            justify-content: space-between;
            gap: 10px;
        }

        button {
            flex: 1;
            padding: 12px;
            font-size: 16px;
            border: none;
            cursor: pointer;
            border-radius: 8px;
            transition: 0.3s;
            font-weight: bold;
        }

        #startPauseBtn { background-color: #28a745; color: white; }
        #startPauseBtn:hover { background-color: #218838; }

        #lapBtn { background-color: #17a2b8; color: white; }
        #lapBtn:hover { background-color: #138496; }

        #resetBtn { background-color: #dc3545; color: white; }
        #resetBtn:hover { background-color: #c82333; }

        #lapBtn:disabled {
            background-color: #555;
            cursor: not-allowed;
        }

        .laps-container {
            max-height: 150px;
            overflow-y: auto;
            margin-top: 15px;
            padding-right: 10px;
        }

        .laps {
            list-style: none;
            padding: 0;
        }

        .laps li {
            background: rgba(255, 255, 255, 0.15);
            padding: 10px;
            margin: 5px 0;
            border-radius: 5px;
            font-size: 16px;
            box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.2);
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Stopwatch</h1>
        <div class="display">00:00:00</div>
        <div class="buttons">
            <button id="startPauseBtn">Start</button>
            <button id="lapBtn" disabled>Lap</button>
            <button id="resetBtn">Reset</button>
        </div>
        <div class="laps-container">
            <ul class="laps"></ul>
        </div>
    </div>

    <script>
        let timer;
        let running = false;
        let elapsedTime = 0;
        let startTime;
        let lapCounter = 1;

        // Selecting elements
        const display = document.querySelector(".display");
        const startPauseBtn = document.getElementById("startPauseBtn");
        const lapBtn = document.getElementById("lapBtn");
        const resetBtn = document.getElementById("resetBtn");
        const lapsContainer = document.querySelector(".laps");

        // Function to format time
        function formatTime(ms) {
            let totalSeconds = Math.floor(ms / 1000);
            let minutes = Math.floor(totalSeconds / 60);
            let seconds = totalSeconds % 60;
            let milliseconds = Math.floor((ms % 1000) / 10);
            return ${String(minutes).padStart(2, "0")}:${String(seconds).padStart(2, "0")}:${String(milliseconds).padStart(2, "0")};
        }

        // Function to update display
        function updateDisplay() {
            elapsedTime = Date.now() - startTime;
            display.textContent = formatTime(elapsedTime);
        }

        // Start/Pause function (Single button)
        function startPause() {
            if (running) {
                clearInterval(timer);
                startPauseBtn.textContent = "Start";
                startPauseBtn.style.backgroundColor = "#28a745";
                lapBtn.disabled = true; // Disable lap button when paused
            } else {
                startTime = Date.now() - elapsedTime;
                timer = setInterval(updateDisplay, 10);
                startPauseBtn.textContent = "Pause";
                startPauseBtn.style.backgroundColor = "#ffcc00";
                lapBtn.disabled = false; // Enable lap button when running
            }
            running = !running;
        }

        // Reset function
        function reset() {
            clearInterval(timer);
            running = false;
            elapsedTime = 0;
            display.textContent = "00:00:00";
            startPauseBtn.textContent = "Start";
            startPauseBtn.style.backgroundColor = "#28a745";
            lapBtn.disabled = true; // Disable lap button after reset
            lapsContainer.innerHTML = "";
            lapCounter = 1;
        }

        // Lap function
        function recordLap() {
            if (running) {
                let lapTime = formatTime(elapsedTime);
                let lapItem = document.createElement("li");
                lapItem.textContent = Lap ${lapCounter++}: ${lapTime};
                lapsContainer.appendChild(lapItem);
            }
        }

        // Keyboard shortcuts
        document.addEventListener("keydown", (event) => {
            if (event.code === "Space") {
                event.preventDefault(); // Prevent scrolling when space is pressed
                startPause();
            }
            if (event.key.toLowerCase() === "r") reset();
            if (event.key.toLowerCase() === "l") recordLap();
        });

        // Event listeners for buttons
        startPauseBtn.addEventListener("click", startPause);
        resetBtn.addEventListener("click", reset);
        lapBtn.addEventListener("click", recordLap);
    </script>
</body>
</html>
