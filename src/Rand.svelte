<!-- 随机数 -->
<script>
    import { onDestroy } from "svelte";

    onDestroy(() => {
        reset();
    });

    let min = 1, max = 20;
    let timer, paused = true, currentRand = 0;
    let lastResults = [];

    let audio = new Audio("static/random.mp3");
        audio.setAttribute("loop", true);

    function rand(min, max){
        return Math.floor(Math.random() * (Math.floor(parseInt(min)) - parseInt(max))) + Math.ceil(parseInt(max));
    }

    function interval(){
        currentRand = rand(min, max);
    }

    // 开始与暂停
    function start() {
        if(paused){
            paused = false;
            if(!timer) timer = setInterval(interval, 100);
            audio.play();
        }
        else{
            paused = true;
            timer = clearInterval(timer);
            audio.pause();

            lastResults= [currentRand, ...lastResults];

            console.log(lastResults);
        }

    }

    // 重置
    function reset() {
        currentRand = 0;
        audio.pause();
        paused = true;
        audio.currentTime = 0;

        lastResults = [];

        timer = clearInterval(timer);
    }

</script>
<div class="tool-content">
    <div class="tool-heading">
        {#if currentRand > 0}
            <span class="number">{currentRand}</span>
        {:else}
            <div class="input-group">
                <input class="input" type="text" min="1" max="3" bind:value={min}/>
                <span>-</span>
                <input class="input" type="text" min="1" max="3" bind:value={max}/>
            </div>
        {/if}
    </div>
    <ul class="tool-reconds">
        {#each lastResults as item}
            <li>{item}</li>
        {/each}
    </ul>
</div>
<div class="tool-foot">
    <div class="foot-control">
        <button on:click={start}>{paused ? (currentRand === 0 ? "开始" : "继续") : "暂停"}</button>
        <button on:click={reset}>重置</button>
    </div>
</div>

<style>
    .tool-heading {
        padding: 2em;
    }

    .tool-heading input {
        height: 1.5em;
    }
</style>
