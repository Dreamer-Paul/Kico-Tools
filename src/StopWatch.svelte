<!-- 秒表 -->
<script>
    import { onDestroy } from "svelte";
    import { countToTime } from "./Tool.svelte";

    onDestroy(() => {
        reset();
    });

    let timer, paused = true, stopped = true;
    let count = 0;
    let displayTime = "00:00.00";
    let lastResults = [];

    function interval() {
        count++;
        displayTime = countToTime(count);
    }

    // 开始与暂停
    function start() {
        if (paused) {
            paused = false;
            stopped = false;

            if (!timer) timer = setInterval(interval, 10);
        }
        else {
            paused = true;
            timer = clearInterval(timer);
        }
    }

    // 重置与计次
    function reset() {
        if (paused) {
            count = 0;
            displayTime = "00:00.00";
            paused = true;
            stopped = true;

            lastResults = [];

            timer = clearInterval(timer);
        }
        else {
            lastResults = [displayTime, ...lastResults];
        }
    }
</script>

<div class="tool-content">
    <div class="tool-heading">
        <span class="number">{displayTime}</span>
    </div>
    <ul class="tool-reconds">
        {#each lastResults as item}
            <li>{item}</li>
        {/each}
    </ul>
</div>
<div class="tool-foot">
    <button on:click={start}>{stopped ? "开始" : (paused ? "继续" : "暂停")}</button>
    <button on:click={reset}>{paused ? "重置" : "计次"}</button>
</div>
