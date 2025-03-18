<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GJETT HVEM</title>
    <style>
        :root {
            --primary: #2c3e50;
            --secondary: #3498db;
            --accent: #1abc9c;
            --background: #f8f9fa;
            --card-bg: #ffffff;
            --text: #2c3e50;
            --shadow: 0 8px 30px rgba(0, 0, 0, 0.1);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', 'Roboto', sans-serif;
        }

        body {
            background-color: var(--background);
            color: var(--text);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 2rem;
        }

        header {
            background: var(--primary);
            color: white;
            width: 100%;
            max-width: 1400px;
            text-align: center;
            padding: 1.5rem;
            border-radius: 20px 20px 0 0;
            margin-bottom: 0;
        }

        #game-container {
            display: flex;
            flex-direction: column;
            max-width: 1400px;
            width: 100%;
        }

        #game-area {
            display: flex;
            gap: 2rem;
            background: var(--card-bg);
            border-radius: 0 0 20px 20px;
            padding: 2rem;
            box-shadow: var(--shadow);
        }

        #grid {
            flex: 1;
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(110px, 1fr));
            gap: 15px;
            background: rgba(44, 62, 80, 0.03);
            padding: 1rem;
            border-radius: 15px;
        }

        .character-card {
            aspect-ratio: 3/4;
            background: var(--card-bg);
            border-radius: 15px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
            transition: all 0.3s ease;
            cursor: pointer;
            overflow: hidden;
            position: relative;
        }

        .character-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.1);
        }

        .character-card.flipped {
            opacity: 0.5;
        }

        .character-card.selected {
            border: 3px solid var(--accent);
        }

        .character-card img {
            width: 100%;
            height: 80%;
            object-fit: cover;
            border-radius: 15px 15px 0 0;
        }

        .character-card .name {
            height: 20%;
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            padding: 5px;
            font-size: 0.9rem;
            font-weight: 500;
        }

        #selected-display {
            width: 300px;
            display: flex;
            flex-direction: column;
            align-items: center;
            background: rgba(44, 62, 80, 0.03);
            border-radius: 15px;
            padding: 1.5rem;
        }

        #selected-character {
            width: 100%;
            aspect-ratio: 3/4;
            max-width: 250px;
            background: var(--card-bg);
            border-radius: 15px;
            box-shadow: var(--shadow);
            overflow: hidden;
            margin-bottom: 1.5rem;
        }

        #selected-character img {
            width: 100%;
            height: 80%;
            object-fit: cover;
            border-radius: 15px 15px 0 0;
        }

        #selected-character .name {
            height: 20%;
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            padding: 10px;
            font-size: 1.2rem;
            font-weight: 500;
        }

        .btn {
            background: var(--accent);
            color: white;
            border: none;
            border-radius: 8px;
            padding: 12px 20px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 500;
            transition: all 0.2s ease;
            width: 100%;
            margin-bottom: 1rem;
        }

        .btn:hover {
            background: #16a085;
            transform: translateY(-2px);
        }

        #reset-btn {
            background: var(--secondary);
        }

        #reset-btn:hover {
            background: #2980b9;
        }

        .message {
            background: rgba(26, 188, 156, 0.1);
            color: var(--accent);
            padding: 10px;
            border-radius: 8px;
            margin-top: 1rem;
            text-align: center;
            font-weight: 500;
        }

        @media (max-width: 1000px) {
            #game-area {
                flex-direction: column;
            }
            
            #selected-display {
                width: 100%;
                flex-direction: row;
                justify-content: center;
                flex-wrap: wrap;
            }
            
            #selected-character {
                max-width: 200px;
                margin-right: 1.5rem;
            }
            
            .button-group {
                display: flex;
                flex-direction: column;
                justify-content: center;
                min-width: 150px;
            }
        }

        @media (max-width: 600px) {
            body {
                padding: 0;
            }
            
            header, #game-area {
                border-radius: 0;
            }
            
            #grid {
                grid-template-columns: repeat(auto-fill, minmax(80px, 1fr));
                gap: 10px;
                padding: 0.5rem;
            }
            
            .character-card .name {
                font-size: 0.8rem;
            }
            
            #selected-display {
                flex-direction: column;
            }
            
            #selected-character {
                margin-right: 0;
            }
        }
    </style>
</head>
<body>

<header>
    <h1>GUESS WHO</h1>
</header>

<div id="game-container">
    <div id="game-area">
        <div id="grid">
            <!-- Character cards will be populated here by JavaScript -->
        </div>
        <div id="selected-display">
            <div id="selected-character">
                <p class="message">Select a character from the grid</p>
            </div>
            <div class="button-group">
                <button id="random-btn" class="btn">Random Character</button>
                <button id="reset-btn" class="btn">Reset Game</button>
            </div>
        </div>
    </div>
</div>

<script>
    document.addEventListener('DOMContentLoaded', function() {
        const grid = document.getElementById('grid');
        const selectedCharacter = document.getElementById('selected-character');
        const randomBtn = document.getElementById('random-btn');
        const resetBtn = document.getElementById('reset-btn');
        
        // Example character data - using the same data from your code
        const characters = [
            { id: 1, name: "Ariyen terrorist 1", image: "taliban.jpg" },
            { id: 2, name: "Haider terrorist 2", image: "erer.jpg" },
            { id: 3, name: "Suhaib pirat", image: "Skjermbilde.jpg" },
            { id: 4, name: "Eren", image: "eren.jpg" },
            { id: 5, name: "Jace", image: "jace.jpg" },
            { id: 6, name: "Petter griffin", image: "petter.jpg" },
            { id: 7, name: "Tanjiro", image: "anime kid.jpg" },
            { id: 8, name: "Astarion", image: "Haider ahh spill.jpg" },
            { id: 9, name: "Shrek", image: "shrek.jpg" },
            { id: 10, name: "Meguru Bachira", image: "fotball.jpg" },
            { id: 11, name: "Bruce wayne", image: "bruce.jpg" },
            { id: 12, name: "steve", image: "steve.jpg" },
            { id: 13, name: "mario", image: "mario.jpg" },
            { id: 14, name: "sonic", image: "sonic.jpg" },
            { id: 15, name: "tony stark", image: "tony.jpg" },
            { id: 16, name: "thanos", image: "thanos.jpg" },
            { id: 17, name: "Airbus A350", image: "placeholder.jpg" },
            { id: 18, name: "BMW M5 comp", image: "placeholder.jpg" },
            { id: 19, name: "Young Sheldon", image: "placeholder.jpg" },
            { id: 20, name: "Patrick Star", image: "placeholder.jpg" },
        ];
        
        let selectedCardId = null;
        
        // Create character cards
        characters.forEach(character => {
            const card = document.createElement('div');
            card.classList.add('character-card');
            card.dataset.id = character.id;
            
            // Using the image paths from your data
            card.innerHTML = `
                <img src="${character.image}" alt="${character.name}" onerror="this.src='https://via.placeholder.com/150?text=${character.name}'">
                <div class="name">${character.name}</div>
            `;
        
            // Add click event to flip cards and select
            card.addEventListener('click', function() {
                // If clicking the selected card, just flip it
                if (this.classList.contains('selected')) {
                    this.classList.toggle('flipped');
                    return;
                }
                
                // Otherwise, select this card
                selectCard(character);
                
                // Update visual selection
                document.querySelectorAll('.character-card').forEach(c => {
                    c.classList.remove('selected');
                });
                this.classList.add('selected');
            });
            
            grid.appendChild(card);
        });
        
        // Function to select a card and display it in the sidebar
        function selectCard(character) {
            selectedCardId = character.id;
            selectedCharacter.innerHTML = `
                <img src="${character.image}" alt="${character.name}" onerror="this.src='https://via.placeholder.com/250?text=${character.name}'">
                <div class="name">${character.name}</div>
            `;
        }
        
        // Random character selection
        randomBtn.addEventListener('click', function() {
            const randomIndex = Math.floor(Math.random() * characters.length);
            const randomCharacter = characters[randomIndex];
            
            // Update selected card
            selectCard(randomCharacter);
            
            // Update visual selection in the grid
            document.querySelectorAll('.character-card').forEach(card => {
                card.classList.remove('selected');
                if (card.dataset.id == randomCharacter.id) {
                    card.classList.add('selected');
                }
            });
        });
        
        // Reset game
        resetBtn.addEventListener('click', function() {
            // Clear selection
            selectedCardId = null;
            selectedCharacter.innerHTML = `<p class="message">Select a character from the grid</p>`;
            
            // Clear all flipped and selected cards
            document.querySelectorAll('.character-card').forEach(card => {
                card.classList.remove('flipped', 'selected');
            });
        });
    });
</script>
</body>
</html>
