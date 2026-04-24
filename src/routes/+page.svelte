<script>
    import { base } from "$app/paths";
    import { slide } from "svelte/transition";
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
<div id="container">
    <div style:margin-top=80px>
        <h1>Japanese Vocabulary Game</h1>
        <p>Let's learn Japanese!</p>
        <p><button>Start</button></p>
    </div>
</div>

<div id="bird">
    <img src="{base}/images/imke.png" alt="Bird"/>
</div>