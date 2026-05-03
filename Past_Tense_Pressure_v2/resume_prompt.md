Save State / Resume Prompt Template

Copy the text inside the box below and paste it into a new chat whenever you need to switch computers, refresh the chat, or get unstuck. Be sure to fill in the bracketed [ ] information!

Hi, you are my coding tutor. We are building an ESL web game from scratch using HTML, Tailwind CSS, and JavaScript. 

**TUTORING INSTRUCTIONS:**
1. Do NOT just give me the final code to copy/paste. 
2. Explain concepts step-by-step and quiz me to ensure I understand the "why".
3. Provide assignment instructions detailed enough so I know *what* to do, but let me figure out *how* to write the code. Scaffold the learning.
4. At the end of every assignment you give me, provide a "Save State Footer" that summarizes our current step so I can copy it into my HTML file.

Here is the Master Curriculum we are following: 
[🛠️ Master Curriculum: The "Smart" ESL Game From Scratch

Philosophy: HTML is the skeleton (structure). CSS is the skin/clothes (looks). JavaScript is the muscles/brain (action and logic). We will build them in that order.

🏗️ Phase 1: The Blueprint (HTML & CSS)

We start with a blank file and build the visual interface of the game.

Module 0: The Workshop. Downloading VS Code, creating an index.html file, and learning how to preview it in your browser.

Module 1: The Skeleton (HTML). Learning basic tags (<div>, <button>, <header>). We will rough out the 4-row layout (Header, Timer, Board, Scoreboard).

Module 2: The Paint (Tailwind CSS). How to use CSS classes to add colors, margins, and center our text without writing complex CSS files.

Module 3: Responsive Design. Using Viewport units (vw/vh) so your game looks perfect on both a 1080p classroom projector and a 4K monitor.

🧠 Phase 2: The Brain (JavaScript Basics)

Now we bring the dead HTML to life.

Module 4: Variables & Arrays. How to store the 200 Q&A pairs and team names in the computer's memory.

Module 5: The DOM (Document Object Model). How JavaScript "talks" to HTML. You will write code that clicks a button and changes the text on the screen.

Module 6: Generating the Board. Writing a for loop that automatically creates 25 cards on the screen so you don't have to copy/paste HTML 25 times.

🎲 Phase 3: The Game Logic

We add the rules, the math, and the timers.

Module 7: Clicking & Flipping. Adding Event Listeners so when you click a card, it flips and shows the question overlay.

Module 8: Math & Scoreboards. Writing functions to add 10 points for a correct answer, or half points for a steal, and updating the UI.

Module 9: The Countdown. Building the 3-minute timer and making it beep using the Web Audio API.

Module 10: Special Cards. Using Math.random() to calculate the weighted odds for the Prizes and Penalties (donating, stealing from 1st place, etc.).

🤖 Phase 4: The AI Upgrade

Transitioning from a traditional game to a "Smart" CALL App.

Module 11: Local Storage. Writing code to save your personal API key securely to your Windows profile so you don't have to paste it every day.

Module 12: The Fetch. Writing an asynchronous function to send the student's answer to the Google Gemini AI.

Module 13: JSON Translation. Reading the AI's response to automatically award points and show grammar feedback on the screen.

Module 14: Final Polish. Bug hunting and preparing the file to be uploaded to GitHub Pages for the world to play!
]

Here is the Original "Blueprint" Code of the game we are trying to recreate:
[<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Past-Tense Pressure | ESL Game</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom Animations & Base Overrides */
        body { margin: 0; padding: 0; overflow: hidden; background-color: #111827; display: flex; justify-content: center; align-items: center; height: 100vh; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        
        #app-container {
            width: 100vw;
            height: 100vh;
            max-width: 177.78vh; /* 16:9 Aspect Ratio Lock */
            max-height: 56.25vw; /* 16:9 Aspect Ratio Lock */
            background-color: #f3f4f6;
            container-type: size;
            position: relative;
            overflow: hidden;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
        }

        .row-grid { display: grid; grid-template-rows: 10% 10% 60% 20%; height: 100%; width: 100%; }
        
        /* 3D Flip Card Styles */
        .perspective-1000 { perspective: 1000px; }
        .transform-style-3d { transform-style: preserve-3d; }
        .backface-hidden { backface-visibility: hidden; }
        .rotate-y-180 { transform: rotateY(180deg); }
        .flip-transition { transition: transform 0.6s cubic-bezier(0.4, 0.2, 0.2, 1); }
        
        .card-front { background-color: white; color: #1e3a8a; border: 0.2vw solid #93c5fd; }
        .card-back { background-color: #f1f5f9; transform: rotateY(180deg); }
        
        /* Overlay Animation */
        .overlay-enter { animation: zoomIn 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards; }
        @keyframes zoomIn {
            from { transform: scale(0.5); opacity: 0; }
            to { transform: scale(1); opacity: 1; }
        }

        /* Timer Alerts */
        .pulse-red { animation: pulseRed 1s infinite; background-color: #991b1b !important; color: white !important;}
        @keyframes pulseRed {
            0%, 100% { background-color: #991b1b; }
            50% { background-color: #ef4444; }
        }
        
        .shake-anim { animation: shake 0.4s ease-in-out infinite; }
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-1cqw); }
            75% { transform: translateX(1cqw); }
        }

        /* Text Sizing Helpers */
        .text-giant { font-size: 8cqw; line-height: 1; }
        .text-huge { font-size: 6cqw; line-height: 1.1; }
        .text-large { font-size: 4cqw; line-height: 1.2; }
        .text-medium { font-size: 2cqw; }
        .text-small { font-size: 1.2cqw; }
    </style>
</head>
<body class="bg-gray-900 select-none">

    <div id="app-container">
        <div class="row-grid">
            <!-- ROW 1: HEADER -->
            <header class="bg-blue-800 text-white flex items-center justify-center shadow-md z-10">
                <h1 class="text-huge font-bold tracking-wider">Past-Tense Pressure</h1>
            </header>

        <!-- ROW 2: GAME STATUS -->
        <div class="grid grid-cols-2 shadow-sm z-10">
            <div id="active-team-display" class="bg-gray-400 text-white flex items-center justify-center text-large font-extrabold transition-colors duration-500">
                Setup Mode
            </div>
            <div id="timer-display" class="bg-slate-800 text-white flex items-center justify-center text-giant font-mono tracking-widest transition-colors duration-300">
                03:00
            </div>
        </div>

        <!-- ROW 3: GAME BOARD (3x Height) -->
        <main class="relative bg-blue-50 p-4 flex items-center justify-center overflow-hidden" id="board-container">
            
            <!-- Start Button -->
            <button id="start-btn" class="absolute z-20 bg-green-500 hover:bg-green-600 text-white font-bold py-6 px-12 rounded-full text-large shadow-2xl transition transform hover:scale-105">
                Begin New Game
            </button>

            <!-- 5x5 Grid -->
            <div id="cards-grid" class="w-full h-full max-w-[1400px] grid grid-cols-5 grid-rows-5 gap-2 md:gap-4 hidden perspective-1000">
                <!-- Cards injected via JS -->
            </div>

            <!-- Fullscreen Card Overlay (Confined to Row 3 for pedagogical visibility) -->
            <div id="card-overlay" class="absolute inset-4 z-30 bg-white rounded-2xl shadow-2xl flex flex-col items-center justify-center hidden p-8 border-8 border-gray-800 text-center">
                
                <div id="overlay-category" class="text-medium text-gray-500 font-bold uppercase tracking-widest mb-2">Language Question</div>
                
                <div id="overlay-content" class="text-huge font-extrabold text-blue-900 mb-6 w-full px-4 break-words">
                    <!-- Content Goes Here -->
                </div>
                
                <div id="overlay-expected" class="text-medium text-gray-400 italic mb-10 hidden">
                    Expected: <span id="overlay-expected-text" class="font-bold text-gray-600"></span>
                </div>

                <!-- Teacher Controls -->
                <div id="teacher-controls" class="flex gap-8 mt-auto hidden">
                    <button id="btn-incorrect" class="bg-red-500 hover:bg-red-600 text-white font-bold py-4 px-10 rounded-2xl text-large shadow-lg flex items-center gap-4 transition transform hover:scale-105">
                        <svg class="w-[1em] h-[1em]" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M6 18L18 6M6 6l12 12"></path></svg>
                        Incorrect
                    </button>
                    <button id="btn-correct" class="bg-green-500 hover:bg-green-600 text-white font-bold py-4 px-10 rounded-2xl text-large shadow-lg flex items-center gap-4 transition transform hover:scale-105">
                        <svg class="w-[1em] h-[1em]" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M5 13l4 4L19 7"></path></svg>
                        Correct
                    </button>
                </div>

                <!-- Special Card Controls -->
                <div id="special-controls" class="mt-auto hidden">
                    <button id="btn-apply-special" class="bg-purple-600 hover:bg-purple-700 text-white font-bold py-4 px-12 rounded-2xl text-large shadow-lg transition transform hover:scale-105">
                        Apply Effect &rarr;
                    </button>
                </div>

                <!-- Steal Controls -->
                <div id="steal-controls" class="flex flex-col items-center gap-6 mt-auto hidden w-full bg-yellow-100 p-6 rounded-xl border-4 border-yellow-400">
                    <div class="text-large font-bold text-yellow-800">STEAL OPPORTUNITY!</div>
                    <div class="text-medium text-yellow-700">Team <span id="steal-team-name" class="font-extrabold"></span>, answer for 50% points!</div>
                    <div class="flex gap-8">
                        <button id="btn-steal-incorrect" class="bg-red-400 hover:bg-red-500 text-white font-bold py-3 px-8 rounded-xl text-medium shadow">Failed Steal</button>
                        <button id="btn-steal-correct" class="bg-green-500 hover:bg-green-600 text-white font-bold py-3 px-8 rounded-xl text-medium shadow">Successful Steal</button>
                    </div>
                </div>

                <!-- Next Turn Controls -->
                <div id="next-turn-controls" class="mt-auto hidden">
                    <button id="btn-next-turn" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-4 px-12 rounded-2xl text-large shadow-lg transition transform hover:scale-105">
                        Next Turn &rarr;
                    </button>
                </div>

            </div>
        </main>

        <!-- ROW 4: ADMIN & SCOREBOARD -->
        <footer class="bg-gray-200 border-t-4 border-gray-300 flex h-full">
            <!-- Left Admin -->
            <div class="w-[20%] min-w-[120px] border-r-4 border-gray-300 p-2 flex flex-col justify-center gap-3 bg-gray-100">
                <button id="add-team-btn" class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-2 rounded text-medium shadow">Add Team</button>
                <button id="remove-team-btn" class="bg-gray-500 hover:bg-gray-600 text-white font-bold py-2 px-2 rounded text-medium shadow">Remove Team</button>
            </div>
            
            <!-- Right Scoreboard -->
            <div id="scoreboard-container" class="w-[80%] flex-grow flex justify-evenly items-center p-2 gap-2">
                <!-- Team widgets injected here -->
            </div>
        </footer>
        </div>
    </div>

    <!-- AUDIO CONTEXT SCRIPT & GAME LOGIC -->
    <script>
        // --- 1. DATABASE: 200 Q&A Pairs (100 Reg, 100 Irreg) ---
        const qaDatabase = [
            // Regular (1-100)
            { q: "Did you accept the gift?", a: "Yes, I accepted the gift." },
            { q: "Why did he accept it?", a: "He accepted it because he liked it." },
            { q: "What did she add to the food?", a: "She added some water." },
            { q: "Did they add more words?", a: "Yes, they added more words." },
            { q: "Who did you ask for help?", a: "I asked my mother for help." },
            { q: "What did he ask the man?", a: "He asked the man for the time." },
            { q: "What did you bake today?", a: "I baked a big cake." },
            { q: "Did she bake the bread?", a: "Yes, she baked the bread." },
            { q: "Who did they call last night?", a: "They called their friends." },
            { q: "When did he call you?", a: "He called me in the morning." },
            { q: "Did you care about the book?", a: "Yes, I cared about the book." },
            { q: "Why did she care so much?", a: "She cared because it was important." },
            { q: "What did he clean yesterday?", a: "He cleaned the entire house." },
            { q: "When did you clean the room?", a: "I cleaned the room on Sunday." },
            { q: "Why did she close the door?", a: "She closed it because it was cold." },
            { q: "Did he close the window?", a: "Yes, he closed the window." },
            { q: "What did they cook for us?", a: "They cooked a good meal." },
            { q: "Where did you cook the fish?", a: "I cooked the fish outside." },
            { q: "Why did the baby cry?", a: "The baby cried for milk." },
            { q: "Did he cry after the game?", a: "No, he did not cry." },
            { q: "Where did she dance?", a: "She danced at the school." },
            { q: "Did they dance together?", a: "Yes, they danced all night." },
            { q: "What did you drop on the floor?", a: "I dropped my pen." },
            { q: "Did he drop the hot pan?", a: "Yes, he dropped the hot pan." },
            { q: "When did the show end?", a: "The show ended at night." },
            { q: "Did the game end well?", a: "Yes, the game ended well." },
            { q: "What did you enjoy the most?", a: "I enjoyed the music the most." },
            { q: "Did she enjoy the long walk?", a: "Yes, she enjoyed the long walk." },
            { q: "How did he fix the car?", a: "He fixed it with his tools." },
            { q: "Did you fix the broken door?", a: "Yes, I fixed the broken door." },
            { q: "Who did the dog follow?", a: "The dog followed the boy." },
            { q: "Did you follow the lines?", a: "Yes, I followed the lines." },
            { q: "What did she guess?", a: "She guessed the right number." },
            { q: "Did he guess your name?", a: "Yes, he guessed my name." },
            { q: "What happened to the tree?", a: "The tree fell down." },
            { q: "When did it happen?", a: "It happened in the morning." },
            { q: "Who did you help today?", a: "I helped an old man." },
            { q: "Did she help with the work?", a: "Yes, she helped me work." },
            { q: "What did they hope for?", a: "They hoped for a good day." },
            { q: "Did you hope to see her?", a: "Yes, I hoped to see her." },
            { q: "How high did he jump?", a: "He jumped over the box." },
            { q: "Did the cat jump on the bed?", a: "Yes, the cat jumped on the bed." },
            { q: "Who did the mother kiss?", a: "She kissed her little boy." },
            { q: "Did he kiss the dog?", a: "No, he did not kiss the dog." },
            { q: "Why did you laugh so hard?", a: "I laughed at the funny joke." },
            { q: "Did they laugh at the picture?", a: "Yes, they laughed at it." },
            { q: "What did she like about it?", a: "She liked the red color." },
            { q: "Did you like the old house?", a: "Yes, I liked the old house." },
            { q: "What did he listen to?", a: "He listened to a song." },
            { q: "Did you listen to the teacher?", a: "Yes, I listened to the teacher." },
            { q: "Where did they live before?", a: "They lived in the city." },
            { q: "Did he live near the water?", a: "Yes, he lived near the water." },
            { q: "What did you look at?", a: "I looked at the big sky." },
            { q: "Did she look under the bed?", a: "Yes, she looked under the bed." },
            { q: "What did he love to do?", a: "He loved to play outside." },
            { q: "Did they love the food?", a: "Yes, they loved the food." },
            { q: "Who did you miss the most?", a: "I missed my family." },
            { q: "Did she miss the bus?", a: "Yes, she missed the bus." },
            { q: "Where did you move the box?", a: "I moved the box to the right." },
            { q: "Did they move to a new home?", a: "Yes, they moved to a new home." },
            { q: "What did the plant need?", a: "The plant needed more water." },
            { q: "Did you need some help?", a: "Yes, I needed some help." },
            { q: "Who opened the big box?", a: "My father opened the big box." },
            { q: "Did she open the letter?", a: "Yes, she opened the letter." },
            { q: "What did he paint yesterday?", a: "He painted a beautiful picture." },
            { q: "Did they paint the walls blue?", a: "No, they painted the walls red." },
            { q: "What game did you play?", a: "We played a fun game." },
            { q: "Did she play with the children?", a: "Yes, she played with them." },
            { q: "What did the horse pull?", a: "The horse pulled the cart." },
            { q: "Did you pull the rope hard?", a: "Yes, I pulled the rope hard." },
            { q: "Why did he push the door?", a: "He pushed it to open it." },
            { q: "Did they push the car?", a: "Yes, they pushed the heavy car." },
            { q: "When did it rain?", a: "It rained all through the night." },
            { q: "Did it rain on your trip?", a: "Yes, it rained on my trip." },
            { q: "What did she show you?", a: "She showed me a book." },
            { q: "Did he show you the way?", a: "Yes, he showed me the way." },
            { q: "Why did the girl smile?", a: "She smiled because she was happy." },
            { q: "Did you smile for the picture?", a: "Yes, I smiled for the picture." },
            { q: "When did the movie start?", a: "The movie started at eight." },
            { q: "Did it start to rain?", a: "Yes, it started to rain." },
            { q: "Why did the car stop?", a: "The car stopped for the animal." },
            { q: "Did you stop to eat?", a: "Yes, I stopped to eat." },
            { q: "Who did you talk to?", a: "I talked to my good friend." },
            { q: "Did they talk about the test?", a: "Yes, they talked about the test." },
            { q: "Who did she thank?", a: "She thanked the kind man." },
            { q: "Did you thank him for his help?", a: "Yes, I thanked him for his help." },
            { q: "What did you try to do?", a: "I tried to read the words." },
            { q: "Did he try the new food?", a: "Yes, he tried the new food." },
            { q: "What did you use to write?", a: "I used a blue pen." },
            { q: "Did she use the computer?", a: "Yes, she used the computer." },
            { q: "How long did you wait?", a: "I waited for a long time." },
            { q: "Did they wait by the door?", a: "Yes, they waited by the door." },
            { q: "Where did he walk today?", a: "He walked to the park." },
            { q: "Did you walk alone?", a: "No, I walked with a friend." },
            { q: "What did the boy want?", a: "The boy wanted a toy." },
            { q: "Did she want more food?", a: "Yes, she wanted more food." },
            { q: "What did he wash outside?", a: "He washed the dirty car." },
            { q: "Did you wash your hands?", a: "Yes, I washed my hands." },
            { q: "Where did they work last year?", a: "They worked on a farm." },
            { q: "Did she work hard today?", a: "Yes, she worked very hard." },

            // Irregular (101-200)
            { q: "Where were you yesterday?", a: "I was at home yesterday." },
            { q: "Was she happy with it?", a: "Yes, she was very happy." },
            { q: "What did he become?", a: "He became a great teacher." },
            { q: "Did it become dark quickly?", a: "Yes, it became dark quickly." },
            { q: "When did the class begin?", a: "The class began in the morning." },
            { q: "Did the song begin softly?", a: "Yes, the song began softly." },
            { q: "Who broke the glass?", a: "The young boy broke the glass." },
            { q: "Did she break her phone?", a: "Yes, she broke her phone." },
            { q: "What did you bring me?", a: "I brought you some water." },
            { q: "Did they bring the dog?", a: "Yes, they brought the dog." },
            { q: "What did they build?", a: "They built a tall building." },
            { q: "Did he build a fire?", a: "Yes, he built a hot fire." },
            { q: "What did she buy at the store?", a: "She bought some fresh milk." },
            { q: "Did you buy a new book?", a: "Yes, I bought a new book." },
            { q: "Who caught the ball?", a: "The tall man caught the ball." },
            { q: "Did he catch the fast train?", a: "No, he did not catch it." },
            { q: "Which one did you choose?", a: "I chose the red one." },
            { q: "Did she choose a name?", a: "Yes, she chose a nice name." },
            { q: "When did your father come?", a: "My father came last night." },
            { q: "Did they come to the house?", a: "Yes, they came to the house." },
            { q: "How much did it cost?", a: "It cost five dollars." },
            { q: "Did the trip cost a lot?", a: "Yes, the trip cost a lot." },
            { q: "What did you cut with the knife?", a: "I cut the soft bread." },
            { q: "Did he cut his finger?", a: "Yes, he cut his finger." },
            { q: "What did you do yesterday?", a: "I did my school work." },
            { q: "Did she do a good job?", a: "Yes, she did a great job." },
            { q: "What did the child draw?", a: "The child drew a round face." },
            { q: "Did you draw a line?", a: "Yes, I drew a straight line." },
            { q: "What did he drink with his meal?", a: "He drank cold water." },
            { q: "Did they drink all the milk?", a: "Yes, they drank all the milk." },
            { q: "Where did you drive the car?", a: "I drove the car to work." },
            { q: "Did she drive fast?", a: "No, she drove very slowly." },
            { q: "What did they eat for lunch?", a: "They ate fish and rice." },
            { q: "Did you eat the whole apple?", a: "Yes, I ate the whole apple." },
            { q: "Why did the tree fall?", a: "The tree fell because of the wind." },
            { q: "Did she fall down the stairs?", a: "Yes, she fell down the stairs." },
            { q: "How did you feel after the run?", a: "I felt very tired." },
            { q: "Did he feel the cold air?", a: "Yes, he felt the cold air." },
            { q: "What did you find under the table?", a: "I found a lost shoe." },
            { q: "Did she find her keys?", a: "Yes, she found her keys." },
            { q: "Where did the bird fly?", a: "The bird flew over the house." },
            { q: "Did they fly across the sea?", a: "Yes, they flew across the sea." },
            { q: "What did he forget to do?", a: "He forgot to close the door." },
            { q: "Did you forget my name?", a: "No, I did not forget it." },
            { q: "What did you get for your birthday?", a: "I got a new watch." },
            { q: "Did she get the letter?", a: "Yes, she got the letter." },
            { q: "What did he give to you?", a: "He gave me a good book." },
            { q: "Did they give you enough time?", a: "Yes, they gave me enough time." },
            { q: "Where did you go yesterday?", a: "I went to the big store." },
            { q: "Did she go with him?", a: "Yes, she went with him." },
            { q: "How tall did the plant grow?", a: "The plant grew very tall." },
            { q: "Did you grow up in the city?", a: "No, I grew up on a farm." },
            { q: "What did you have for dinner?", a: "I had meat and potatoes." },
            { q: "Did he have a good time?", a: "Yes, he had a great time." },
            { q: "What did you hear outside?", a: "I heard a loud noise." },
            { q: "Did she hear the music?", a: "Yes, she heard the music." },
            { q: "Where did the boy hide?", a: "The boy hid under the bed." },
            { q: "Did you hide the money?", a: "Yes, I hid the money." },
            { q: "What did the ball hit?", a: "The ball hit the window." },
            { q: "Did he hit the tree?", a: "Yes, he hit the large tree." },
            { q: "What did she hold in her hand?", a: "She held a small bird." },
            { q: "Did you hold the baby?", a: "Yes, I held the baby softly." },
            { q: "Where did you keep the box?", a: "I kept the box in my room." },
            { q: "Did they keep their promise?", a: "Yes, they kept their promise." },
            { q: "How did you know the answer?", a: "I knew it from a book." },
            { q: "Did he know the way home?", a: "Yes, he knew the way home." },
            { q: "When did they leave the house?", a: "They left early in the morning." },
            { q: "Did she leave her bag behind?", a: "Yes, she left her bag behind." },
            { q: "What did he lose at the park?", a: "He lost his new coat." },
            { q: "Did you lose the game?", a: "Yes, we lost the game." },
            { q: "What did she make for us?", a: "She made a warm coat." },
            { q: "Did you make a mistake?", a: "Yes, I made a small mistake." },
            { q: "Where did you meet your friend?", a: "I met my friend at school." },
            { q: "Did they meet the president?", a: "No, they did not meet him." },
            { q: "How much did you pay for it?", a: "I paid ten dollars." },
            { q: "Did she pay the man?", a: "Yes, she paid the man." },
            { q: "Where did he put the keys?", a: "He put the keys on the table." },
            { q: "Did you put the book away?", a: "Yes, I put the book away." },
            { q: "What book did you read?", a: "I read a book about animals." },
            { q: "Did she read the whole story?", a: "Yes, she read the whole story." },
            { q: "What did you ride today?", a: "I rode a strong horse." },
            { q: "Did they ride the bus?", a: "Yes, they rode the bus." },
            { q: "How fast did he run?", a: "He ran very fast." },
            { q: "Did you run all the way?", a: "Yes, I ran all the way here." },
            { q: "What did the teacher say?", a: "The teacher said to sit down." },
            { q: "Did she say hello to you?", a: "Yes, she said hello to me." },
            { q: "What did you see in the water?", a: "I saw a large fish." },
            { q: "Did they see the beautiful sky?", a: "Yes, they saw the beautiful sky." },
            { q: "What did he sell to you?", a: "He sold me his old car." },
            { q: "Did she sell all her books?", a: "Yes, she sold all her books." },
            { q: "What did you send to her?", a: "I sent her a long letter." },
            { q: "Did they send the money?", a: "Yes, they sent the money." },
            { q: "Where did you sit in class?", a: "I sat in the front row." },
            { q: "Did he sit on the chair?", a: "Yes, he sat on the hard chair." },
            { q: "How long did you sleep?", a: "I slept for eight hours." },
            { q: "Did the baby sleep well?", a: "Yes, the baby slept very well." },
            { q: "Who did you speak to?", a: "I spoke to the kind woman." },
            { q: "Did she speak English?", a: "Yes, she spoke clear English." },
            { q: "Where did they swim?", a: "They swam in the blue sea." },
            { q: "Did you swim fast?", a: "Yes, I swam very fast." }
        ];

        // --- 2. STATE MANAGEMENT & GAME LOGIC ---
        const colors = [
            { bg: 'bg-red-500', hover: 'hover:bg-red-600', hex: '#ef4444', lightHex: '#fca5a5', darkHex: '#7f1d1d' },
            { bg: 'bg-blue-500', hover: 'hover:bg-blue-600', hex: '#3b82f6', lightHex: '#93c5fd', darkHex: '#1e3a8a' },
            { bg: 'bg-green-500', hover: 'hover:bg-green-600', hex: '#22c55e', lightHex: '#86efac', darkHex: '#14532d' },
            { bg: 'bg-yellow-500', hover: 'hover:bg-yellow-600', hex: '#eab308', lightHex: '#fde047', darkHex: '#713f12' },
            { bg: 'bg-purple-500', hover: 'hover:bg-purple-600', hex: '#a855f7', lightHex: '#d8b4fe', darkHex: '#581c87' },
            { bg: 'bg-pink-500', hover: 'hover:bg-pink-600', hex: '#ec4899', lightHex: '#f9a8d4', darkHex: '#831843' }
        ];

        let teams = [
            { id: 1, name: "Team 1", score: 0, color: colors[0] },
            { id: 2, name: "Team 2", score: 0, color: colors[1] }
        ];

        let currentTeamIndex = 0;
        let isPlaying = false;
        let deck = [];
        let remainingQA = [];
        let activeCard = null;
        let activeCardDOM = null;
        
        // Timer State
        const TIMER_MAX = 180; // 3 minutes
        let timeLeft = TIMER_MAX;
        let timerInterval = null;
        let stealMode = false;
        let audioCtx = null;
        let doublePointsActive = false;

        // --- 3. DOM ELEMENTS ---
        const startBtn = document.getElementById('start-btn');
        const gridContainer = document.getElementById('cards-grid');
        const activeTeamDisplay = document.getElementById('active-team-display');
        const timerDisplay = document.getElementById('timer-display');
        const scoreboardContainer = document.getElementById('scoreboard-container');
        
        const cardOverlay = document.getElementById('card-overlay');
        const overlayCategory = document.getElementById('overlay-category');
        const overlayContent = document.getElementById('overlay-content');
        const overlayExpected = document.getElementById('overlay-expected');
        const overlayExpectedText = document.getElementById('overlay-expected-text');
        
        const teacherControls = document.getElementById('teacher-controls');
        const specialControls = document.getElementById('special-controls');
        const stealControls = document.getElementById('steal-controls');
        const stealTeamName = document.getElementById('steal-team-name');
        const nextTurnControls = document.getElementById('next-turn-controls');

        // --- 4. AUDIO BEEP GENERATOR ---
        function initAudio() {
            if (!audioCtx) {
                const AudioContext = window.AudioContext || window.webkitAudioContext;
                if (AudioContext) audioCtx = new AudioContext();
            }
        }
        
        function playBeep() {
            if (!audioCtx) return;
            try {
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.type = 'square';
                osc.frequency.setValueAtTime(880, audioCtx.currentTime); // A5 note
                gain.gain.setValueAtTime(0.1, audioCtx.currentTime); // Gentle volume
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                osc.start();
                osc.stop(audioCtx.currentTime + 0.15);
            } catch (e) {}
        }

        // --- 5. INITIALIZATION & SETUP ---
        document.addEventListener('DOMContentLoaded', () => {
            renderTeams();
            updateActiveTeamDisplay();
            
            // Events
            document.getElementById('add-team-btn').addEventListener('click', addTeam);
            document.getElementById('remove-team-btn').addEventListener('click', removeTeam);
            startBtn.addEventListener('click', startGame);
            
            document.getElementById('btn-correct').addEventListener('click', () => handleScore(true));
            document.getElementById('btn-incorrect').addEventListener('click', () => handleScore(false));
            document.getElementById('btn-apply-special').addEventListener('click', applySpecialEffect);
            
            document.getElementById('btn-steal-correct').addEventListener('click', () => handleStealScore(true));
            document.getElementById('btn-steal-incorrect').addEventListener('click', () => handleStealScore(false));
            document.getElementById('btn-next-turn').addEventListener('click', hideOverlay);
            
            // Initialize user interaction for audio context policy
            document.body.addEventListener('click', initAudio, { once: true });
        });

        function addTeam() {
            if (teams.length >= 6) return; // Max 6 teams for layout
            const newId = teams.length + 1;
            teams.push({
                id: newId,
                name: `Team ${newId}`,
                score: 0,
                color: colors[teams.length % colors.length]
            });
            renderTeams();
        }

        function removeTeam() {
            if (teams.length <= 2) return; // Min 2 teams
            teams.pop();
            renderTeams();
        }

        function renderTeams() {
            scoreboardContainer.innerHTML = '';
            
            // Dynamic text scaling to prevent truncation when more teams are added
            const nameFontSize = teams.length > 4 ? `${16 / teams.length}cqw` : '4cqw';
            const scoreFontSize = teams.length > 4 ? `${24 / teams.length}cqw` : '6cqw';

            teams.forEach((t, i) => {
                const widget = document.createElement('div');
                // Changed justify-between to justify-center to move the text up and away from the bottom border
                widget.className = `flex-1 min-w-0 flex flex-col items-center justify-center p-2 rounded-xl shadow-md h-[90%] transition border-b-8`;
                widget.style.borderBottomColor = t.color.lightHex;
                widget.style.backgroundColor = (isPlaying && i === currentTeamIndex) ? '#f0fdf4' : 'white';
                
                widget.innerHTML = `
                    <input type="text" value="${t.name}" onchange="updateTeamName(${i}, this.value)" 
                           class="font-bold text-center w-full bg-transparent outline-none text-gray-800 text-ellipsis overflow-hidden whitespace-nowrap transition-all duration-300 mb-1"
                           style="font-size: ${nameFontSize}; line-height: 1.2;"
                           ${isPlaying ? 'readonly' : ''}>
                    <div class="font-extrabold leading-none transition-all duration-300 mt-1" style="color: ${t.color.darkHex}; font-size: ${scoreFontSize};">${t.score}</div>
                `;
                scoreboardContainer.appendChild(widget);
            });
        }

        function updateTeamName(index, name) {
            teams[index].name = name;
            if (index === currentTeamIndex) updateActiveTeamDisplay();
        }

        function updateActiveTeamDisplay() {
            if (teams.length === 0) return;
            const t = teams[currentTeamIndex];
            activeTeamDisplay.textContent = isPlaying ? `TURN: ${t.name}` : 'SETUP MODE';
            activeTeamDisplay.className = `flex items-center justify-center text-large font-extrabold transition-colors duration-500 ${isPlaying ? t.color.bg : 'bg-gray-400'} text-white shadow-inner`;
        }

        // --- 6. DECK GENERATION & CARD LOGIC ---
        function generateDeck() {
            // Filter DB into Yes/No Questions and Wh-Questions
            let ynDB = qaDatabase.filter(item => /^(Did|Was|Were)\b/i.test(item.q)).sort(() => 0.5 - Math.random());
            let whDB = qaDatabase.filter(item => !/^(Did|Was|Were)\b/i.test(item.q)).sort(() => 0.5 - Math.random());
            
            deck = [];
            
            // 5 Cards: Y/N Question, Need Answer
            for (let i = 0; i < 5; i++) {
                deck.push({
                    type: 'qa-question',
                    display: ynDB[i].q,
                    expected: ynDB[i].a,
                    used: false
                });
            }

            // 5 Cards: Wh- Question, Need Answer
            for (let i = 0; i < 5; i++) {
                deck.push({
                    type: 'qa-question',
                    display: whDB[i].q,
                    expected: whDB[i].a,
                    used: false
                });
            }
            
            // 5 Cards: Y/N Answer, Need Question
            for (let i = 5; i < 10; i++) {
                deck.push({
                    type: 'qa-answer',
                    display: ynDB[i].a,
                    expected: ynDB[i].q,
                    used: false
                });
            }

            // 5 Cards: Wh- Answer, Need Question
            for (let i = 5; i < 10; i++) {
                deck.push({
                    type: 'qa-answer',
                    display: whDB[i].a,
                    expected: whDB[i].q,
                    used: false
                });
            }
            
            // Store remaining questions for Double Points mechanic
            remainingQA = [...ynDB.slice(10), ...whDB.slice(10)].sort(() => 0.5 - Math.random());

            // 5 Special Cards
            for (let i = 0; i < 2; i++) deck.push(generatePenalty());
            for (let i = 0; i < 2; i++) deck.push(generatePrize());
            deck.push(generateWildcard());
            
            // Shuffle Deck
            deck.sort(() => 0.5 - Math.random());
        }

        function generatePenalty() {
            const r = Math.random();
            let desc = "", effectCode = "";
            if (r < 0.30) { desc = "Lose 5 Points!"; effectCode = "lose5"; }
            else if (r < 0.45) { desc = "Lose 10 Points!"; effectCode = "lose10"; }
            else if (r < 0.75) { desc = "Donate 5 Pts to Last Place!"; effectCode = "don5"; }
            else if (r < 0.90) { desc = "Donate 10 Pts to Last Place!"; effectCode = "don10"; }
            else if (r < 0.95) { desc = "Go to Last Place (-5 of lowest)!"; effectCode = "golast"; }
            else { desc = "Skip Turn!"; effectCode = "skip"; }
            return { type: 'special', category: 'PENALTY', display: desc, effectCode, used: false, color: 'text-red-600' };
        }

        function generatePrize() {
            const r = Math.random();
            let desc = "", effectCode = "";
            if (r < 0.30) { desc = "Gain 5 Points!"; effectCode = "gain5"; }
            else if (r < 0.45) { desc = "Gain 10 Points!"; effectCode = "gain10"; }
            else if (r < 0.75) { desc = "Steal 5 Pts from 1st Place!"; effectCode = "steal5"; }
            else if (r < 0.90) { desc = "Steal 10 Pts from 1st Place!"; effectCode = "steal10"; }
            else if (r < 0.95) { desc = "Go to 1st Place (+5 of highest)!"; effectCode = "gofirst"; }
            else { desc = "Answer a Question for Double Points!"; effectCode = "double"; }
            return { type: 'special', category: 'PRIZE', display: desc, effectCode, used: false, color: 'text-green-600' };
        }

        function generateWildcard() {
            const r = Math.random();
            let desc = "", effectCode = "";
            if (r < 0.50) { desc = "Wildcard: Skip Turn!"; effectCode = "skip"; }
            else { desc = "Wildcard: Answer a Question for Double Points!"; effectCode = "double"; }
            return { type: 'special', category: 'WILDCARD', display: desc, effectCode, used: false, color: 'text-purple-600' };
        }

        // --- 7. GAME PLAY & UI RENDERING ---
        function startGame() {
            initAudio();
            isPlaying = true;
            currentTeamIndex = 0;
            doublePointsActive = false;
            teams.forEach(t => t.score = 0);
            
            generateDeck();
            
            startBtn.classList.add('hidden');
            gridContainer.classList.remove('hidden');
            
            renderBoard();
            updateActiveTeamDisplay();
            renderTeams();
            resetTimerUI();
        }

        function renderBoard() {
            gridContainer.innerHTML = '';
            deck.forEach((card, index) => {
                const cardEl = document.createElement('div');
                cardEl.className = `relative w-full h-full cursor-pointer perspective-1000 group`;
                
                // Inner 3D Wrapper
                const innerEl = document.createElement('div');
                innerEl.className = `w-full h-full transform-style-3d flip-transition relative rounded-xl shadow-lg ${card.used ? 'rotate-y-180 opacity-50 cursor-not-allowed' : 'hover:scale-105'}`;
                
                // Front
                const frontEl = document.createElement('div');
                frontEl.className = `absolute inset-0 backface-hidden card-front flex items-center justify-center rounded-xl text-huge font-bold shadow-md`;
                frontEl.textContent = index + 1;
                
                // Back (Pattern)
                const backEl = document.createElement('div');
                backEl.className = `absolute inset-0 backface-hidden card-back flex items-center justify-center rounded-xl bg-gray-200 border-4 border-gray-300`;
                backEl.innerHTML = `<svg class="w-1/2 h-1/2 text-gray-400" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd"></path></svg>`;

                innerEl.appendChild(frontEl);
                innerEl.appendChild(backEl);
                cardEl.appendChild(innerEl);
                
                if (!card.used) {
                    cardEl.addEventListener('click', () => openCard(index, innerEl));
                }
                
                gridContainer.appendChild(cardEl);
            });
        }

        function openCard(index, domElement) {
            if (activeCard || deck[index].used) return;
            
            activeCard = deck[index];
            activeCardDOM = domElement;
            
            // Flip the physical card on the board first
            domElement.classList.add('rotate-y-180');
            deck[index].used = true;
            
            setTimeout(() => {
                showOverlay(activeCard);
            }, 400); // Wait for flip animation
        }

        function showOverlay(card) {
            stealMode = false;
            cardOverlay.classList.remove('hidden');
            cardOverlay.classList.add('overlay-enter');
            
            overlayExpected.classList.add('hidden');
            teacherControls.classList.add('hidden');
            specialControls.classList.add('hidden');
            stealControls.classList.add('hidden');
            nextTurnControls.classList.add('hidden');

            if (card.type === 'special') {
                overlayCategory.textContent = card.category;
                overlayCategory.className = `text-large font-bold tracking-widest mb-4 ${card.color}`;
                overlayContent.textContent = card.display;
                overlayContent.className = `text-giant font-extrabold mb-8 ${card.color} w-full px-4 break-words drop-shadow-md`;
                specialControls.classList.remove('hidden');
            } else {
                let catText = card.type === 'qa-question' ? "Provide the Answer:" : "Provide the Question:";
                if (doublePointsActive) catText += " (DOUBLE POINTS!)";
                overlayCategory.textContent = catText;
                
                overlayCategory.className = `text-medium text-blue-600 font-bold uppercase tracking-widest mb-4 ${doublePointsActive ? 'text-purple-600 animate-pulse' : ''}`;
                overlayContent.textContent = card.display;
                overlayContent.className = `text-huge font-extrabold text-blue-900 mb-6 w-full px-4 break-words leading-tight`;
                
                overlayExpectedText.textContent = card.expected;
                teacherControls.classList.remove('hidden');
                
                startTimer();
            }
        }

        function hideOverlay() {
            if (activeCard && activeCard.type !== 'special') {
                doublePointsActive = false;
            }
            
            cardOverlay.classList.add('hidden');
            cardOverlay.classList.remove('overlay-enter');
            activeCard = null;
            activeCardDOM.classList.add('opacity-50', 'cursor-not-allowed'); // Gray out used card
            activeCardDOM = null;
            stopTimer();
            resetTimerUI();
            nextTurn();
        }

        // --- 8. TIMER LOGIC ---
        function startTimer(duration = TIMER_MAX) {
            timeLeft = duration;
            updateTimerDisplay();
            timerDisplay.classList.remove('pulse-red', 'shake-anim');
            
            clearInterval(timerInterval);
            timerInterval = setInterval(() => {
                timeLeft--;
                updateTimerDisplay();
                
                if (timeLeft === 30) {
                    timerDisplay.classList.add('pulse-red');
                }
                
                if (timeLeft <= 10 && timeLeft > 0) {
                    timerDisplay.classList.add('shake-anim');
                    playBeep();
                }

                if (timeLeft <= 0) {
                    stopTimer();
                    timerDisplay.textContent = "00:00";
                    timerDisplay.classList.remove('shake-anim');
                    // Automatically mark incorrect if time runs out
                    if (!stealMode) handleScore(false); 
                    else handleStealScore(false);
                }
            }, 1000);
        }

        function stopTimer() {
            clearInterval(timerInterval);
            timerDisplay.classList.remove('pulse-red', 'shake-anim');
        }

        function updateTimerDisplay() {
            let m = Math.floor(timeLeft / 60).toString().padStart(2, '0');
            let s = (timeLeft % 60).toString().padStart(2, '0');
            timerDisplay.textContent = `${m}:${s}`;
        }
        
        function resetTimerUI() {
            timeLeft = TIMER_MAX;
            updateTimerDisplay();
            timerDisplay.classList.remove('pulse-red', 'shake-anim');
        }

        // --- 9. SCORING & SPECIAL EFFECTS ---
        function concludeTurn() {
            teacherControls.classList.add('hidden');
            stealControls.classList.add('hidden');
            overlayExpected.classList.remove('hidden');
            nextTurnControls.classList.remove('hidden');
        }

        function handleScore(isCorrect) {
            stopTimer();
            const team = teams[currentTeamIndex];
            
            if (isCorrect) {
                team.score += doublePointsActive ? 20 : 10;
                concludeTurn();
            } else {
                // Trigger Steal
                stealMode = true;
                teacherControls.classList.add('hidden');
                stealControls.classList.remove('hidden');
                
                // Identify next team for steal
                const stealTeamIdx = (currentTeamIndex + 1) % teams.length;
                stealTeamName.textContent = teams[stealTeamIdx].name;
                
                // Give 30 seconds for steal
                startTimer(30);
            }
        }

        function handleStealScore(isCorrect) {
            stopTimer();
            if (isCorrect) {
                const stealTeamIdx = (currentTeamIndex + 1) % teams.length;
                teams[stealTeamIdx].score += doublePointsActive ? 10 : 5; // 50% points for steal
            }
            concludeTurn(); // Show answer for 5s then pass turn
        }

        function applySpecialEffect() {
            const team = teams[currentTeamIndex];
            const eff = activeCard.effectCode;
            
            // Helpers for First/Last place Steals & Donates
            const uniqueScoresDesc = [...new Set(teams.map(t => t.score))].sort((a,b) => b - a);
            const uniqueScoresAsc = [...uniqueScoresDesc].reverse();

            // Find Steal Targets (First place, or Second place if active team is solely First)
            let stealScore = uniqueScoresDesc[0];
            let stealTargets = teams.filter(t => t.score === stealScore && t.id !== team.id);
            if (stealTargets.length === 0 && uniqueScoresDesc.length > 1) {
                stealScore = uniqueScoresDesc[1];
                stealTargets = teams.filter(t => t.score === stealScore);
            }

            // Find Donate Targets (Last place, or Second to last if active team is solely Last)
            let donateScore = uniqueScoresAsc[0];
            let donateTargets = teams.filter(t => t.score === donateScore && t.id !== team.id);
            if (donateTargets.length === 0 && uniqueScoresAsc.length > 1) {
                donateScore = uniqueScoresAsc[1];
                donateTargets = teams.filter(t => t.score === donateScore);
            }

            // Helpers for Go First/Last
            const maxScore = uniqueScoresDesc[0];
            const minScore = uniqueScoresAsc[0];
            const teamsAtMax = teams.filter(t => t.score === maxScore);
            const teamsAtMin = teams.filter(t => t.score === minScore);

            switch(eff) {
                case 'lose5': team.score -= 5; break;
                case 'lose10': team.score -= 10; break;
                case 'don5': 
                    team.score -= 5; 
                    if(donateTargets.length > 0) {
                        donateTargets.forEach(t => t.score += (5 / donateTargets.length));
                    }
                    break;
                case 'don10': 
                    team.score -= 10; 
                    if(donateTargets.length > 0) {
                        donateTargets.forEach(t => t.score += (10 / donateTargets.length));
                    }
                    break;
                case 'golast': 
                    if (team.score === minScore) {
                        if (teamsAtMin.length > 1) team.score -= 5;
                    } else {
                        team.score = minScore - 5;
                    }
                    break;
                case 'gain5': team.score += 5; break;
                case 'gain10': team.score += 10; break;
                case 'steal5': 
                    team.score += 5; 
                    if(stealTargets.length > 0) {
                        stealTargets.forEach(t => t.score -= (5 / stealTargets.length));
                    }
                    break;
                case 'steal10': 
                    team.score += 10; 
                    if(stealTargets.length > 0) {
                        stealTargets.forEach(t => t.score -= (10 / stealTargets.length));
                    }
                    break;
                case 'gofirst': 
                    if (team.score === maxScore) {
                        if (teamsAtMax.length > 1) team.score += 5;
                    } else {
                        team.score = maxScore + 5;
                    }
                    break;
                case 'double': 
                    if (remainingQA.length > 0) {
                        doublePointsActive = true;
                        let extraQ = remainingQA.pop();
                        activeCard.type = 'qa-answer';
                        activeCard.display = extraQ.a;
                        activeCard.expected = extraQ.q;
                        showOverlay(activeCard);
                        return; // Prevent hideOverlay() from being called
                    }
                    break; 
                case 'skip': break; // Do nothing, skips turn
            }
            
            // Clean up fractions for UI presentation (rounds to 1 decimal place, e.g. 3.3 instead of 3.333...)
            teams.forEach(t => {
                t.score = Math.round(t.score * 10) / 10;
            });
            
            hideOverlay();
        }

        function nextTurn() {
            currentTeamIndex = (currentTeamIndex + 1) % teams.length;
            updateActiveTeamDisplay();
            renderTeams();
            
            // Check if game is over (all cards flipped)
            if (deck.every(c => c.used)) {
                endGame();
            }
        }

        function endGame() {
            isPlaying = false;
            renderTeams();
            
            activeTeamDisplay.textContent = "GAME OVER";
            activeTeamDisplay.className = "flex items-center justify-center text-large font-extrabold transition-colors duration-500 bg-gray-800 text-white shadow-inner";
            
            let winner = [...teams].sort((a,b) => b.score - a.score)[0];
            
            gridContainer.classList.add('hidden');
            startBtn.classList.remove('hidden');
            startBtn.textContent = `${winner.name} WINS! Play Again?`;
            startBtn.className = `absolute z-20 ${winner.color.bg} ${winner.color.hover} text-white font-bold py-8 px-16 rounded-full text-giant shadow-2xl transition transform hover:scale-105`;
        }
    </script>
</body>
</html>
]

Here is my index.html code so far:
[Paste your current index.html code here]

[Choose one:]
- What is my next assignment based on my index.html file? 
- I am submitting my assignment for review, how does it look?
- I am getting a bug/error on line X, can you help me fix it without just giving me the answer?
