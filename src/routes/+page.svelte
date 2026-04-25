<script>
    import { base } from "$app/paths";
    import { fade, fly, slide } from "svelte/transition";
    import { onMount } from "svelte";

    // 48% of these words will be tested in pre-post
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
            "ちゃ",
            "Tea"
        ]
    ]

    let gameStatus = $state(0);
    let answerStatus = $state(0);

    let round = $state({
        "curr": "",
        "corr": "",
        "choices": []
    });

    function generateRound() {
        round.choices = [];
        let rand = Math.floor((Math.random() * japanese.length));
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
        console.log(round.choices);
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
            setTimeout(()=> {answerStatus = 0; gameStatus = 2; generateRound()}, 2500);
            setTimeout(function() {gameStatus = 1}, 3000);
        }
        else {
            setTimeout(function() {correctEmote = false}, 500)
            //console.log(correct);
            setTimeout(()=> {answerStatus = 0; gameStatus = 2; generateRound()}, 3500);
            setTimeout(function() {gameStatus = 1; correctEmote = true}, 4000);
        }
    }

    onMount(() => {
        generateRound();
    })

</script>
<svelte:head>
    <link rel="preload" as="image" href="{base}/images/imke.png"/>
    <link rel="preload" as="image" href="{base}/images/imkeAngry.png"/>
</svelte:head>
<style>
    #container {
        position: fixed;
        transform: translate(-50%, -50%);
        left: 50%;
        top: 50%;
        padding: 20px;
        background-color: rgb(136, 61, 33);
        box-shadow: 0px 0px 20px 10px rgba(127, 91, 24, 0.544);
        border-radius: 20px;
        width: 90%;
        height: 80%;
        z-index: 999;
    }
    #bird {
        z-index: 1000;
        position: fixed;
        transform: translate(-50%, 0%);
        left: 50%;
        bottom: -90px;
        animation: pulse 3s infinite ease-in-out;
        img {
            max-width: 300px;
        }
    }
    @keyframes pulse {
        0%, 100% {
            bottom: -90px;
        }
        50% {
            bottom: -110px;
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
</style>
<div id="container">
    <div style:margin-top=60px>
        {#if gameStatus == 0}
        <div out:fade>
            <h1>Japanese Vocabulary Game</h1>
            <p>Let's learn Japanese!</p>
            <p><button onclick={function() {gameStatus = 1;}}>Start</button></p>
        </div>
        {/if}
        {#if gameStatus == 1}
        <div style:text-align="center" in:fly={{delay: 1000, y:100}} out:fade>
            <h1 translate="no" style:user-select="none">{round.curr}</h1>
            <h2 style:color="white">Translate this word into its English equivalent</h2>
            <div id="choices">
                {#each round.choices as x}
                    {#if x == round.corr}
                    <button class:correct={answerStatus == 1} disabled={answerStatus == 1} onclick={() => {progress(true)}}>{x}</button>
                    {:else}
                    <button class:incorrect={answerStatus == 1} disabled={answerStatus == 1} onclick={() => {progress(false)}}>{x}</button>
                    {/if}
                {/each}
            </div>
        </div>
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