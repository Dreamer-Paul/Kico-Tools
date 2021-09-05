<!-- 时钟 -->
<script>
    import { onMount } from "svelte";
    import { getTime } from "./Tool.svelte";

    let time, currentTime = "00:00:00";
    let hour, min, sec;

    onMount(() => {
        time = getTime();

        currentTime = time[0];
        hour = (time[1] * 30) + (time[2] / 2);
        min = (time[2] * 6) + (time[3] / 10);
        sec = time[3] * 6;

        const interval = setInterval(() => {
            time = getTime();

            currentTime = time[0];
            hour = (time[1] * 30) + (time[2] / 2);
            min = (time[2] * 6) + (time[3] / 10);
            sec = time[3] * 6;
        }, 1000);

        return () => clearInterval(interval);
    });
</script>

<div class="tool-content">
    <div class="tool-notice"></div>
    <div class="tool-clock">
        <div class="clock-sec" style="transform: rotate({sec}deg)"></div>
        <div class="clock-min" style="transform: rotate({min}deg)"></div>
        <div class="clock-hour" style="transform: rotate({hour}deg)"></div>
    </div>
    <div class="tool-heading">
        <time>{currentTime}</time>
    </div>
    <ul class="tool-reconds">

    </ul>
</div>
<div class="tool-foot">
    <div class="foot-control">
        <button>添加闹钟</button>
    </div>
</div>
