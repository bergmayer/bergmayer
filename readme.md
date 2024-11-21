<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Guided by Voices Quiz</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 20px;
        }
        .button {
            padding: 10px 20px;
            margin: 10px;
            font-size: 16px;
            cursor: pointer;
        }
        .hidden {
            display: none;
        }
        .score {
            margin-top: 20px;
            font-size: 18px;
        }
    </style>
</head>
<body>
    <h1>Guided by Voices Quiz</h1>
    <p>Can you guess if the title is real or fake?</p>

    <div id="start-screen">
        <button class="button" onclick="startQuiz()">Begin</button>
    </div>

    <div id="quiz" class="hidden">
        <p id="category"></p>
        <p id="title"></p>
        <button class="button" onclick="checkAnswer(true)">Real</button>
        <button class="button" onclick="checkAnswer(false)">Fake</button>
    </div>

    <div id="results" class="hidden">
        <h2>Quiz Complete!</h2>
        <p>Your Score:</p>
        <p>Correct: <span id="correct-score"></span></p>
        <p>Wrong: <span id="wrong-score"></span></p>
    </div>

    <script>
        // Arrays for real and fake titles
        const realAlbumTitles = ["Devil Between My Toes","Sandbox","Self-Inflicted Aerial Nostalgia","Same Place the Fly Got Smashed","Propeller","Vampire on Titus","Bee Thousand","Alien Lanes","Under the Bushes Under the Stars","Mag Earwhig!","Do the Collapse","Isolation Drills","Universal Truths and Cycles","Earthquake Glue","Half Smiles of the Decomposed","Let’s Go Eat the Factory","Class Clown Spots a UFO","The Bears for Lunch","English Little League","Motivational Jumpsuit","Cool Planet","Please Be Honest","August by Cake","How Do You Spell Heaven","Space Gun","Zeppelin Over China","Warp and Woof","Sweating the Plague","Surrender Your Poppy Field","Mirrored Aztec","Styles We Paid For","Earth Man Blues","It’s Not Them. It Couldn’t Be Them. It Is Them!","Crystal Nuns Cathedral","Tremblers and Goggles by Rank","Scalping the Guru","La La Land","Welshpool Frillies","Nowhere to Go but Up","Strut of Kings"];

const fakeAlbumTitles = ["Echoes of the Elastic Horizon","Skeleton Anthem","Magnetic Candlelight","Velvet Satellites","Paperclip Serenade","Holographic Aviators","Infinite Carousel","The Fractured Telegram","Whispered Choirs","Twilight for the Albatross","Lanterns and Mirrors","Static Cathedral","Fossil of a Velvet Day","Transparent Blues","Stained Glass Reveries","The Clockwork Collapse","Moonlit Aviator Songs","Glass Parades and Plastic Stars","Drifting Through the Fogline","Anagram Chronicles","Canaries in the Static","Polaroid Echoes","Chariots of Mercury","Parallel Riddles","The Factory Lament","Ghostlight in Stereo","The Accordion Prophet","Infinite Radio Memoirs","Broken Orbit Reveries","Monarchs of the Invisible","The Amplifier’s Last Waltz","Sundown Cartographers","Obsolete Aurora","Nocturnal Blueprint","Gravity Bells","The Binary Transmission","Accordion’s Ghost","The Temporary Aviator","Postcards to the Horizon","Reflections of the Tinfoil Sky"];

const realSongNames = ["Postal Blowfish","Pimple Zoo","Club Molluska","How's My Drinking?","Man Called Aerodynamics","Metal Mothers","To Remake the Young Flyer","Local Mix-Up / Murder Charge","Bulldog Skin","Circus World","Acorns & Orioles","A Visit to the Creep Doctor","Useless Inventions","On the Tundra","Mammoth Cave","In Stitches","Liar's Tale","Surgical Focus","#2 in the Model Home Series","Drag Days","Alex Bell","Airshow '88","Vote for Me Dummy","Sons of Apollo","See My Field","Ghosts of a Different Dream","Red Gas Circle","Sad if I Lost It","I Love Kangaroos","Some Drilling Implied","Non-Absorbing","The Future Is in Eggs","Skills Like This","Jane of the Waking Universe","Sheetkickers","Serene King","Rhine Jive Click","Atom Eyes","Underwater Explosions","Colonel Paper","No Sky","Order for the New Slave Trade","Sleep Over Jack","Dodging Invisible Rays","Blatant Doom Trip","Bright Paper Werewolves","Dragons Awake!","Matter Eater Lad","Lethargy","Lord of Overstock","Not Behind the Fighter Jet","Old Battery","That's Good","The Closets of Henry","Particular Damaged","Sot","Kisses to the Crying Cooks","Sister I Need Wine","Hey Mr. Soundman","Office of Hearts","Choking Tara","Hank's Little Fingers","Unspirited","Planet Score","Electronic Windows to Nowhere","Do the Earth","Jabberstroker","Blink Blank","Marchers in Orange","Ergo Space Pig","We've Got Airplanes","Lights Out in Memphis (Egypt)","Trap Soul Door","Storm Vibrations","Liquid Indian","Discussing Wallace Chambers","Liars' Box","Chocolate Boy","It's Baseball","Sport Component National","Long Distance Man","Crystal Nuns Cathedral","Donkey School","Navigating Flood Regions","Unstable Journey","Cyclops","Mr. Child","The Great Blake Street Canoe Race","Scissors","Ark Technician","Key Losers","Perfect This Time","Beekeeper Seeks Ruth","Ambergris","Heavy Metal Country","How Can a Plumb Be Perfected?","At Odds with Dr. Genesis","Zoo Pie","Take to the Sky","Starboy","Sing for Your Meat","I Am Produced","Now to War","Megaphone Riley","Goodbye Note","Gonna Never Have to Die","Sopor Joe","Pivotal Film","The Very Second","Crash at Lake Placebo","Angelic Weirdness","Grey Spat Matters","King Flute","Wire Greyhounds","My Thoughts Are a Gas","I'll Replace You with Machines","A Portrait Destroyed by Fire","Man Called Blunder","Beat Your Wings","Chief Barrel Belly","Crutch Came Slinking","Johnny Appleseed","Much Better Mr. Buckles","Taco","Buffalo","Birddog and Jesus","Sentimental Wars","Bunco Men","Street Party","Doughnut for a Snowman","Privately","5° on the Inside","Lizard on the Red Brick Wall","Superior Sector Janitor X","Overloaded","They Don't Play the Drums Anymore","Deathtrot and Warlock Riding a Rooster","Indian Fables","Cheyenne","Free Agents","Eye City","Please Be Honest","Frostman","Pink Drink","Now I'm Crying","Sunshine Girl Hello","Hudson Rake","Apology in Advance","The Other Place","Lips of Steel","Asia Minor","Perhaps Now the Vultures","Damn Good Mr. Jam","The Tumblers","Flight Advantage","Land of Danger","Immortals","Ego Central High","Asphyxiated Circle","Girl from the Sun","Crocker's Favorite Song","Math Rock","Little Lines","Your Cricket Is Rather Unique","White Whale","Mushroom Art","Pantherz (Demo)","Skin Parade","World of Fun","I Bet Hippy","Razor Bug","Sometimes I Cry","Hey Aardvark","Ha Ha Man","Stabbing a Star","An Earful o' Wax","Dr. Feelgood Falls Off the Ocean","Good Morning Sir","Optional Bases Opposed","She Lives in an Airport","Keep It in Motion","Cool Off Kid Kilowatt","Always Gone","Amusement Park Is Over","Can't Hear the Revolution","Excited Ones","Re-Develop","Cocksoldiers and Their Postwar Stubble","Is She Ever?","My Secretary","My Country","Dog's Out","Subspace Biographies","Crunch Pillow","Canteen Plums","Queen Parking Lot","Little Jimmy the Giant","The Terrible Two","Pretty Bombs","Unfun Glitz","Climbing a Ramp","A Contest Featuring Human Beings","Made Man","When We All Hold Hands at the End of the World","My Zodiac Companion","Wrecking Now","Get to Know the Ropes","Dust Devil","Supermarket the Moon","Cul-De-Sac Kids","Planet's Own Brand","Wormhole","Dance of Gurus","Endless Seafood","Stops","Daily Get-Ups","Trendspotter Acrobat","2nd Moves to Twin","The Birthday Democrats","What About It?","The Top Chick's Silver Chord","Please Freeze Me","Sudden Fiction","Margaret Middle School","The Main Street Wizards","At the Farms","Dirty Kid School","Fountain of Youth","Let's Ride","The Drinking Jim Crow","Murphy Had a Birthday","Can't Stop","Fantasy Creeps","It's Food","He's the Uncle","Eureka Signs","Break Even","A Trophy Mule in Particular","She Goes Off at Night","Arthur Has Business Elsewhere","A Second Spurt of Growth","E-5","I Think I Had It","I Think I Have It Again","Scalding Creek","Evolution Circus","I Certainly Hope Not","Mother's Milk Elementary","Keep Me Down","Short on Posters","Cohesive Scoops","Birds in the Pipe","Sons of the Beard","An Unmarketed Product","Mix Up the Satellite","Kiss Only the Important Ones","Released Into Dementia","Fictional Environment Dream","The Kissing Life","Dusty Bushworms"];

const fakeSongNames = ["Lanterns Over Cardboard Town","Chimney Sweep Rebellion","Figures in Glue","The Apricot Consensus","Periscopes for the Blindfolded","Engine Oil Confession","Baritone Shuffle","Static on the Ghostline","Window Dressing Prophets","Plums for the Paper Admiral","Snare Drum Apparition","Electric Graveyard Picnic","Lemon Freeze Doctrine","Crumbling Milk Tower","Parlor of the Albatross","Syntax Cannibals","Rocket in Reverse Gear","Gallery of Missing Frames","The Adhesive Dialogue","Faded Photographs of Giants","The Microphone Emperor","Spindrift Valedictorian","Anagram Sisters","Century of Buzzards","Fluorescent Paperbacks","The Velvet Collapse","Chariot Full of Razors","Funeral in the Acid Forest","Daylight Trophies","Hypnotic Tides of Progress","The Amphibian Affair","Rattlesnake Postcard","Sixteen Envelopes in Fire","Porcelain Inhaler","Gentlemen of the Fog","Evaporated Choirboys","The Candlestick Consensus","Anthems for Diminished Stars","Sprinklers in the Afterlife","Crisis of the Crystal King","Neon Telegrams","Thirteen Ghosts and a Fender","Nightmare in Velvet Ink","Treetop Operators","Holographic Hive Mind","Slingshot Astronomy","Amber Wings Black Horizon","Monkfish Kingdom","The Temporary Messiah","Parade of Hollow Giants","Firecracker Vestibule","Seashell Orchestra","A Village of Rubber Keys","Clockwork Satellites","Eyelid Lightning","Coffee Stains on Saturn","The Glue Factory Standoff","Canaries in a Meltdown","Blueprints for the Ascension","Rusted Halos in Bloom","An Inkwell for Two Minds","Polaroids of the Eclipse","Transparent Leviathan","Broken Orbit Teaspoon","Static Windshield Wipers","Chromatic Descent","Mushroom Cloud Whistle","Lost in the Factory Cathedral","Sculptors of Red Sand","Yellow Jackets in Stereo","Lighthouse Cannons","Marionettes for the Riot","Songs for the Frozen Eclipse","The Glass City Wager","Turquoise Aviators","Fractured Submarine","Drums of the Liquid Cathedral","The Trembling Kaleidoscope","Infinite Fishhooks","Postcards from the Collapse","Cellophane Universe","Blizzard of Mercury","The Petrified Garden","Spiral Eyeball Reflections","Preachers in the Tornado","Ballad of the Hexagon Driver","Obsolete Symphony","A Fable of Rusted Wings","Windmill in a Hurricane Jar","Fossils of an Invisible Era","Magnetized Canyon","Pinecone Melancholy","Echoes from a Burnt House","The Accordion Mirage","Spies in the Broken Mirror","Golden Shadows in Reverse","Carpenters of the Cloud Age","Venetian Blindfold Riddles","Black Currant Alarm Clock","Throne of Porcelain Teeth","Astronauts with Paper Wings","Digital Vultures in the Morning","Magnetized Cathedral","The Elastic Burial","Skyhands Operate the Machine","Motel of the Divine Eye","Cinderblock Serenade","Pencils for the Pyre","Interrupting the Binary Heart","Hornet Geometry","Tinfoil for Astronauts","Turbulent Era Waltz","Whiskers in the Reactor","Cluster of Orange Stars","Absent-Minded Moonlighters","Voices Behind the Curtain Wire","Televised Cornfields","An Optimist in Ruins","Silver Tongue Forecast","Parachute Morning","Anchors of Cannons","Pinecone without Garden","Chromatic against Reactor","Liquefied on Astronauts","Wires beyond Burial","Porcelain on Exchange","Spiral Over Dust","Lanterns Over Eclipse","Accordion against Vestibule","Sculptors of Serenade","Transparent Dreams for the Riot","Golden Fishhooks in the Reactor","Broken Balloons to the Abyss","Golden Satellites under the Glass Ceiling","Velvet Forests in a Spiral Storm","Amber Forests on the Rainbow","Invisible Satellites in a Mirage","Silent Wizards of Diminished Stars","Faded Clouds with Amber Wings","Faded Giants of Diminished Stars","Infinite Statues in a Hurricane Jar","Noisy Tornadoes on the Rainbow","Shimmering Statues in the Fog","Blazing Whistles of the Eclipse","Ceramic Whistles in a Spiral Storm","Faded Choirboys of the Eclipse","Frozen Canvases of the Eclipse","Frozen Wizards with Amber Wings","Endless Wizards of Diminished Stars","Frozen Fables against the Machine","Hollow Candlesticks on the Rainbow","Amber Rivers on the Rainbow","Broken Tornadoes of Diminished Stars","Crystal Aviators in the Tinfoil Orchestra","Frozen Choirboys in the Reactor","Golden Balloons in Reverse","Magnetic Forests from a Burnt House","Velvet Balloons in the Tinfoil Orchestra","Spiral Nightmares on the Cloud Age","Broken Horizons for the Hollow Sky","Silent Forests of the Eclipse","Hollow Giants in a Mirage","Velvet Machines to the Abyss","Transparent Fossils on Saturn","Forgotten Tornadoes in the Tinfoil Orchestra","Crystal Rivers of Plastic Dreams","Transparent Balloons of Diminished Stars","Fractured Rivers of Plastic Dreams","Blazing Staircases in a Hurricane Jar","Radiant Staircases in a Mirage","Infinite Anvils in the Crystal Kingdom","Hollow Paperbacks in a Hurricane Jar","Silent Horizons of Cannons","Noisy Lanterns from a Burnt House","Crystal Cathedrals on the Rainbow","Broken Whistles in the Tinfoil Orchestra","Falling Anvils of the Paper Admiral","Liquid Rivers in Reverse","Silent Compasses on Cardboard Town","Infinite Clouds of Diminished Stars","Woven Rivers of Plastic Dreams","Synthetic Horizons in the Fog","Golden Clouds in the Fog","Magnetic Compasses in the Factory Cathedral","Shimmering Horizons of Diminished Stars","Silent Dreams to the Abyss","Broken Mirrors of Cannons","Faded Choirboys on the Cloud Age","Faded Compasses in a Hurricane Jar","Silent Whistles of the Eclipse","Rusty Giants in a Hurricane Jar","Liquid Whistles in the Fog","Ceramic Fossils in the Reactor","Dizzy Clouds on the Rainbow","Spiral Cathedrals of Diminished Stars","Liquid Fables under the Glass Ceiling","Faded Paperbacks against the Machine","Rusty Fossils in the Tinfoil Orchestra","Shimmering Anvils of the Afterlife","Velvet Dreams in the Tinfoil Orchestra","Amber Whistles in Reverse","Faded Satellites in a Spiral Storm","Forgotten Tornadoes of the Paper Admiral","Golden Rivers on the Rainbow","Faded Choirboys in a Spiral Storm","Digital Clouds of the Paper Admiral","Transparent Clouds in a Hurricane Jar","Ceramic Aviators in the Tinfoil Orchestra","Synthetic Lanterns in Reverse","Broken Giants of the Afterlife","Amber Staircases against the Machine","Infinite Paperbacks in a Spiral Storm","Hollow Giants of the Eclipse","Broken Compasses on Saturn","Shimmering Canvases of Diminished Stars","Falling Rivers of the Eclipse","Painted Forests to the Abyss","Radiant Aviators under the Glass Ceiling","Crystal Fishhooks from the Collapse","Faded Whistles in the Reactor","Synthetic Wings with Amber Wings","Digital Operators of the Afterlife","Invisible Aviators of the Afterlife","Rusty Machines to the Abyss","Amber Nightmares under the Glass Ceiling","Faded Forests for the Hollow Sky","Infinite Wizards of the Eclipse","Falling Balloons in Reverse","Woven Machines for the Riot","Synthetic Canvases for the Hollow Sky","Shimmering Lanterns on Saturn","Velvet Tornadoes in the Crystal Kingdom","Velvet Canvases against the Machine","Fractured Tornadoes of the Paper Admiral","Fractured Choirboys in the Tinfoil Orchestra","Hollow Statues from the Collapse","Spiral Whistles of the Paper Admiral","Hollow Compasses on Saturn","Electric Whistles from a Burnt House","Silent Satellites of the Eclipse","Frozen Wizards of Diminished Stars","Invisible Rivers on Cardboard Town","Noisy Fishhooks in the Factory Cathedral","Liquid Nightmares of the Paper Admiral","Woven Clouds in the Factory Cathedral","Falling Wings in a Hurricane Jar","Velvet Paperbacks on Cardboard Town","Digital Lanterns in the Factory Cathedral","Broken Fables to the Abyss","Shimmering Horizons for the Hollow Sky","Silent Paperbacks in the Reactor","Shimmering Fossils to the Abyss","Magnetic Operators of Cannons","Falling Aviators in the Fog","Rusty Fishhooks in the Tinfoil Orchestra","Velvet Machines from the Collapse","Synthetic Mirrors against the Machine","Transparent Wings against the Machine","Woven Tornadoes in Reverse","Falling Fossils on the Cloud Age"];

        // Combine all items into one array with category and a flag
        let quizItems = [
            ...realAlbumTitles.map(title => ({ title, isReal: true, category: 'Album Title' })),
            ...fakeAlbumTitles.map(title => ({ title, isReal: false, category: 'Album Title' })),
            ...realSongNames.map(title => ({ title, isReal: true, category: 'Song Name' })),
            ...fakeSongNames.map(title => ({ title, isReal: false, category: 'Song Name' }))
        ];

        // Shuffle quiz items for randomness
        function shuffle(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        }

        let currentQuestion = 0;
        let correctScore = 0;
        let wrongScore = 0;
        let selectedQuestions = [];

        function startQuiz() {
            shuffle(quizItems);
            selectedQuestions = quizItems.slice(0, 20); // Limit to 20 questions
            currentQuestion = 0;
            correctScore = 0;
            wrongScore = 0;

            document.getElementById('start-screen').classList.add('hidden');
            document.getElementById('quiz').classList.remove('hidden');
            loadNextQuestion();
        }

        function loadNextQuestion() {
            if (currentQuestion >= selectedQuestions.length) {
                showResults();
                return;
            }
            const currentItem = selectedQuestions[currentQuestion];
            document.getElementById('category').textContent = `Category: ${currentItem.category}`;
            document.getElementById('title').textContent = currentItem.title;
        }

        function checkAnswer(isRealGuess) {
            const currentItem = selectedQuestions[currentQuestion];
            if (isRealGuess === currentItem.isReal) {
                correctScore++;
            } else {
                wrongScore++;
            }
            currentQuestion++;
            loadNextQuestion();
        }

        function showResults() {
            document.getElementById('quiz').classList.add('hidden');
            document.getElementById('results').classList.remove('hidden');
            document.getElementById('correct-score').textContent = correctScore;
            document.getElementById('wrong-score').textContent = wrongScore;
        }
    </script>
</body>
</html>