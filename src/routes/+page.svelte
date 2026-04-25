<script>
    import { base } from "$app/paths";
    import { fade, fly, slide } from "svelte/transition";
    import { onMount } from "svelte";
    //import { passive } from "svelte/legacy"; <- This autoimported and I'm not sure why

    let japanese = [
        [
            "あか",
            "Red"
        ],
        [
            "あお",
            "Blue"
        ],
        [
            "しち",
            "Seven"
        ],
        [
            "きゅう",
            "Nine"
        ],
        [
            "しろ",
            "White"
        ],
        [
            "なん",
            "Minute"
        ],
        [
            "くろ",
            "Black"
        ],
        [ 
            "こんにちは",
            "Good Afternoon"
        ],
        [
            "ようび",
            "Day of the Week"
        ],
        [
            "きんようび",
            "Friday"
        ],
        [
            "どようび",
            "Saturday"
        ],
        [
            "くがつ",
            "September"
        ],
        [
            "じゅうがつ",
            "October"
        ],
        [
            "フラワー",
            "Flower"
        ],
        [
            "ピンク",
            "Pink"
        ],
        [
            "犬",
            "Dog"
        ],
        [
            "ナイト",
            "Night"
        ],
        [
            "キノコ",
            "Mushroom"
        ],
        [
            "川",
            "River"
        ],
        [
            "ジュース",
            "Juice"
        ],
        [
            "ティー",
            "Tea"
        ],
        [
            "マウンテン",
            "Mountain"
        ],
        [
            "オーシャン", 
            "Ocean"
        ],
        [
            "ボート",
            "Boat"
        ],
        [
            "火",
            "Fire"
        ],
        [
            "木",
            "Tree"
        ],
        [
            "マネー",
            "Money"
        ],
        [
            "ミルク",
            "Milk"
        ],
        [
            "とけい",
            "Clock"
        ],
        [
            "ちいさい",
            "Small"
        ],
        [
            "むずかしい",
            "Difficult"
        ],
        [
            "さかな",
            "Fish"
        ],
        [
            "とりにく",
            "Chicken"
        ]
    ]

    let assessed = [];
    for (let i = 0;  i < japanese.length; i++) {
        assessed.push(i);
    }

    let gameStatus = $state(0);
    let answerStatus = $state(0);

    let round = $state({
        "curr": "",
        "corr": "",
        "choices": []
    });

    function generateRound() {
        if (assessed.length == 0) {
            for (let i = 0;  i < japanese.length; i++) {
                assessed.push(i);
            }
        }
        round.choices = [];
        let rand = Math.floor((Math.random() * assessed.length));
        rand = assessed[rand];
        assessed = assessed.filter((index) => index != rand);
        round.curr = japanese[rand][0];
        round.corr = japanese[rand][1];

        let corrIndex = Math.floor(Math.random() * 4);
        for (let i = 0; i < 4; i++) {
            if (corrIndex == i) {
                round.choices.push(round.corr);
            }
            else {
                let rand2 = Math.floor((Math.random() * japanese.length));
                if (rand2 == rand) {
                    i--;
                    continue;
                }
                else if (checkChoices(japanese[rand2][1])) {
                    round.choices.push(japanese[rand2][1]);
                }
                else {
                    i--; 
                }
            }
        }
        //console.log(round.choices);
    }

    function checkChoices(choice) {
        for (let i = 0; i < round.choices.length; i++) {
            if (round.choices[i] == choice) {
                return false;
            }
        }
        return true;
    }

    let correctEmote = $state(true);
    function progress (correct) {
        setTimeout(function() {answerStatus = 1}, 500);
        if (correct) {
            setTimeout(function() {streak++; correctAnswers++}, 500);
            setTimeout(()=> {answerStatus = 0; gameStatus = 2; generateRound()}, 2500);
            setTimeout(function() {gameStatus = 1}, 3000);
        }
        else {
            setTimeout(function() {correctEmote = false}, 500)
            setTimeout(function() {streak = 0}, 1000)
            //console.log(correct);
            setTimeout(()=> {answerStatus = 0; gameStatus = 2; generateRound()}, 3500);
            setTimeout(function() {gameStatus = 1; correctEmote = true}, 4000);
            streak = 0;
        }
    }

    onMount(() => {
        generateRound();
        console.log(japanese.length + " words are on this game. If questions are completed every 10 seconds, there is an about " + (Math.floor((42/japanese.length)*100) + "% chance each word gets assessed"));
        console.log(Math.floor((12/japanese.length)*100) + "% is tested on pre-post")
    })

    let timerMode = $state(0);
    let timer = $state(420);
    let timerString = $state("");
    onMount(() => {
        window.setInterval(timerCount, 1000);
    })
    function timerCount() {
        if (timerMode == 1) {
            timerString = convertTimerString();
            //console.log(timer);
            timer--;
        }
        if (timer < -1) {
            timerMode = 0;
            gameStatus = 3;
        }
    }
    function convertTimerString() {
        let mins = Math.floor(timer/60);
        let secs = timer%60;
        if (secs <= 9) {
            secs = "0" + secs;
        }
        let str = mins + ":" + secs;
        return str;
    }

    let correctAnswers = $state(0);
    let streak = $state(0);

</script>
<svelte:head>
    <link rel="preload" as="image" href="{base}/images/imke.png"/>
    <link rel="preload" as="image" href="{base}/images/imkeAngry.png"/>
</svelte:head>
<style>
    #container {
        position: fixed;
        transform: translate(-50%, -50%);
        transition: top 0.5s ease-in-out;
        left: 50%;
        top: 50%;
        padding: 20px;
        background-color: rgb(136, 61, 33);
        box-shadow: 0px 0px 20px 10px rgba(127, 91, 24, 0.544);
        border-radius: 20px;
        width: 90%;
        height: 78.5%;
        z-index: 999;
    }
    #container.active {
        top: 53.5%;
    }
    #bird {
        z-index: 1000;
        position: fixed;
        transform: translate(-50%, 0%);
        left: 50%;
        bottom: -100px;
        animation: pulse 3s infinite ease-in-out;
        
        img {
            max-width: 300px;
            transition: transform 0.3s ease-in-out;
        }
    }
    @keyframes pulse {
        0%, 100% {
            bottom: -100px;
        }
        50% {
            bottom: -120px;
        }
    }

    button {
        background-color: white;
        color: rgb(136, 61, 33);
    }

    #choices {
        display: flex;
        justify-content: center;
        button {
            padding: 20px;
            margin: 8px;
            user-select: none;
        }
        button:hover {
            transform: scale(1.05);
        }
        button.correct {
            background-color: lightgreen;
            color: rgb(53, 99, 53);
            transform: scale(1);
        }
        button.incorrect {
            background-color: lightcoral;
            color: rgb(92, 49, 49);
            transform: scale(1);
        }
    }

    button.pause {
        padding: 5px;
        margin: 5px;
        transform: translateY(3px);
        span {
            transform: translateY(2px)
        }
        border-radius: 30px;
    }

    #streak {
        position: fixed;
        transform: translate(-50%, 0%);
        left: 50%;
        background-color: rgb(75, 33, 17);
        padding: 0px 20px;
        border-radius: 20px;
        h3 {
            transform: translateY(-3.5px);

            span {
                transform: translateY(3.5px);
            }
        }
    }
</style>
{#if gameStatus == 1 || gameStatus == 2}
<div id="streak" in:slide={{delay: 700}} out:slide>
    <h3><span class="material-symbols-outlined">check_circle</span> {correctAnswers} <span class="material-symbols-outlined" translate="no">mode_heat</span> {streak}</h3>
</div>
{/if}
<div id="container" class:active={gameStatus == 1 || gameStatus == 2}>
    {#if gameStatus == 1 || gameStatus == 2}
    <div style:margin-top=5px in:fade={{delay: 500}} out:fade>
        <p style:font-weight=800>{timerString} <button class="pause" onclick={() => {if (timerMode == 1) { timerMode = 0} else {timerMode = 1}}}><span class="material-symbols-outlined" translate="no">{#if timerMode}pause_circle{:else}play_circle{/if}</span></button></p>
    </div>
    {/if}
    <div style:margin-top=20px>
        {#if gameStatus == 0}
        <div out:fade style:margin-top=60px>
            <h1>Japanese Vocabulary Game</h1>
            <p>Let's learn Japanese! Don't get questions wrong, or the bird gets angry...</p>
            <p><button onclick={function() {gameStatus = 0.5; setTimeout(() => {gameStatus = 1; timerMode = 1}, 500)}}>Start</button></p>
        </div>
        {/if}
        {#if gameStatus == 1}
        <div style:text-align="center" in:fly={{delay: 1000, y:100}} out:fade>
            <h1 translate="no" style:user-select="none">{round.curr}</h1>
            <h2 style:color="white">Translate this word into its English equivalent</h2>
            <div id="choices">
                {#each round.choices as x}
                    {#if x == round.corr}
                    <button class:correct={answerStatus == 1} disabled={answerStatus == 1 || timerMode == 0} onclick={() => {progress(true)}}>{x}</button>
                    {:else}
                    <button class:incorrect={answerStatus == 1} disabled={answerStatus == 1 || timerMode == 0} onclick={() => {progress(false)}}>{x}</button>
                    {/if}
                {/each}
            </div>
        </div>
        {/if}
        {#if gameStatus == 3}
        <h1>Great job!</h1>
        {/if}
    </div>
</div>

<div id="bird">
    {#if correctEmote}
    <img src="{base}/images/imke.png" alt="Bird"/>
    {:else}
    <img src="{base}/images/imkeAngry.png" alt="Bird"/>
    {/if}
</div>