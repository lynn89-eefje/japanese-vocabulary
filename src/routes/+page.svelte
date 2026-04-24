<script>
    import { base } from "$app/paths";
    import { fade, slide } from "svelte/transition";
    import { onMount } from "svelte";

    let screen = {
        "x": 0,
        "y": 0
    }

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
            "ひる",
            "Afternoon"
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
        ]
    ]

    let gameStatus = 0;

    onMount(() => {
        console.log(japanese);
    })
</script>

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
</style>
<svelte:window></svelte:window>
<div id="container">
    <div style:margin-top=80px>
        {#if gameStatus == 0}
        <div out:fade>
            <h1>Japanese Vocabulary Game</h1>
            <p>Let's learn Japanese!</p>
            <p><button onclick={function() {gameStatus = 1;}}>Start</button></p>
        </div>
        {/if}
    </div>
</div>

<div id="bird">
    <img src="{base}/images/imke.png" alt="Bird"/>
</div>