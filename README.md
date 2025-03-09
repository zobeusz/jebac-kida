<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CS2 Banowanie Map</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; }
        .map { display: inline-block; padding: 10px; margin: 10px; border: 2px solid black; cursor: pointer; transition: all 0.3s ease; }
        .banned { opacity: 0.5; text-decoration: line-through; background-color: red; }
        .selected { border: 3px solid green; }
    </style>
    <script>
        let banHistory = [];
        let timer;
        let audioBan = new Audio('ban.mp3');
        let audioSelect = new Audio('select.mp3');

        function banMap(element) {
            if (!element.classList.contains("banned")) {
                element.classList.add("banned");
                banHistory.push(element.innerText);
                updateOrganizerView();
                checkLastMap();
                updateBanHistory();
                startTimer();
                audioBan.play();
            }
        }
        function resetBans() {
            document.querySelectorAll(".map").forEach(map => map.classList.remove("banned", "selected"));
            banHistory = [];
            updateOrganizerView();
            updateBanHistory();
            document.getElementById("chosenMap").innerText = "Brak";
            clearTimeout(timer);
        }
        function updateOrganizerView() {
            let bannedMaps = [];
            document.querySelectorAll(".map.banned").forEach(map => {
                bannedMaps.push(map.innerText);
            });
            document.getElementById("organizerView").innerText = "Zbanowane mapy: " + (bannedMaps.length ? bannedMaps.join(", ") : "Brak");
        }
        function checkLastMap() {
            let availableMaps = document.querySelectorAll(".map:not(.banned)");
            if (availableMaps.length === 1) {
                availableMaps[0].classList.add("selected");
                document.getElementById("chosenMap").innerText = "Wybrana mapa: " + availableMaps[0].innerText;
                audioSelect.play();
            }
        }
        function updateBanHistory() {
            document.getElementById("banHistory").innerText = "Historia banów: " + (banHistory.length ? banHistory.join(" → ") : "Brak");
        }
        function startTimer() {
            clearTimeout(timer);
            let countdown = 30;
            document.getElementById("timer").innerText = "Czas na wybór: " + countdown + "s";
            timer = setInterval(() => {
                countdown--;
                document.getElementById("timer").innerText = "Czas na wybór: " + countdown + "s";
                if (countdown <= 0) {
                    clearInterval(timer);
                    alert("Czas na wybór minął!");
                }
            }, 1000);
        }
    </script>
</head>
<body>
    <h1>Banowanie Map CS2</h1>
    <button onclick="resetBans()">Reset</button>
    <p id="timer">Czas na wybór: 30s</p>
    <p><a href="#">Link dla kapitanów</a></p>
    
    <h2>Składy drużyn</h2>
    <div>
        <h3>Drużyna 1</h3>
        <ul>
            <li><input type="text" placeholder="Gracz 1"></li>
            <li><input type="text" placeholder="Gracz 2"></li>
            <li><input type="text" placeholder="Gracz 3"></li>
            <li><input type="text" placeholder="Gracz 4"></li>
            <li><input type="text" placeholder="Gracz 5"></li>
        </ul>
        <h3>Drużyna 2</h3>
        <ul>
            <li><input type="text" placeholder="Gracz A"></li>
            <li><input type="text" placeholder="Gracz B"></li>
            <li><input type="text" placeholder="Gracz C"></li>
            <li><input type="text" placeholder="Gracz D"></li>
            <li><input type="text" placeholder="Gracz E"></li>
        </ul>
    </div>
    
    <h2>BO1</h2>
    <div id="bo1">
        <div class="map" onclick="banMap(this)">Mirage</div>
        <div class="map" onclick="banMap(this)">Inferno</div>
        <div class="map" onclick="banMap(this)">Nuke</div>
        <div class="map" onclick="banMap(this)">Overpass</div>
        <div class="map" onclick="banMap(this)">Vertigo</div>
        <div class="map" onclick="banMap(this)">Ancient</div>
        <div class="map" onclick="banMap(this)">Anubis</div>
    </div>
    <h2>Podgląd dla organizatora</h2>
    <p id="organizerView">Zbanowane mapy: Brak</p>
    <h2>Ostatecznie wybrana mapa</h2>
    <p id="chosenMap">Brak</p>
    <h2>Historia banów</h2>
    <p id="banHistory">Brak</p>
</body>
</html>
