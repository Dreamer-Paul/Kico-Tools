<!-- 倒计时 -->
<script>
    import { onDestroy } from "svelte";
    import { clsn, countToTime, timeToCount } from "./Tool.svelte";

    onDestroy(() => {
        reset();
    });

    let min = 0, sec = 30;
    let timer, paused = true, stopped = true;
    let count = 0;
    let displayTime = "00:00.00";

    // 响铃
    const audio = new Audio("static/timer.mp3");
    audio.setAttribute("loop", true);

    let alertVisible = false;

    function interval() {
        if (count > 1) {
            count--;
            displayTime = countToTime(count);
        }
        else {
            count--;
            paused = true;
            stopped = true;
            timer = clearInterval(timer);

            audio.play();

            alertVisible = true;
        }
    }

    // 开始和暂停
    function start() {
        // 从头开始
        if (stopped) {
            paused = false;
            stopped = false;

            count = timeToCount(min, sec);

            if (!timer) timer = setInterval(interval, 10);
        }
        // 继续
        else if (paused) {
            paused = false;

            if (!timer) timer = setInterval(interval, 10);
        }
        // 暂停
        else {
            paused = true;
            timer = clearInterval(timer);
        }

    }

    // 重置
    function reset() {
        count = 0;
        paused = true;
        stopped = true;

        timer = clearInterval(timer);

        audio.pause();
        audio.currentTime = 0;

        alertVisible = false;
    }
</script>

<div class={clsn("tool-notice", alertVisible && "active")}>
    倒计时已到！<button on:click={reset}>取消</button>
</div>
<div class="tool-content">
    <div class="tool-heading">
        {#if stopped && paused}
            <div class="input-group">
                <input class="input" type="number" min="0" max="59" bind:value={min}/>
                <span>分</span>
                <input class="input" type="number" min="0" max="59" bind:value={sec}/>
                <span>秒</span>
            </div>
        {:else}
            <span class="number">{displayTime}</span>
        {/if}
    </div>
</div>
<div class="tool-foot">
    <button on:click={start}>{stopped ? "开始" : (paused ? "继续" : "暂停")}</button>
    <button on:click={reset}>重置</button>
</div>

<style>
    .tool-heading {
        padding: 2em;
    }

    .tool-heading input {
        height: 1.5em;
    }

    .input-group span {
        font-size: 1rem;
    }
</style>
