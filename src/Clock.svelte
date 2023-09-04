<!-- 时钟 -->
<script>
    import { onMount } from "svelte";
    import { clsn, getTime, loadTime, saveTime, zeroFill } from "./Tool.svelte";
    import AddClock from "./components/clock/AddClock.svelte";

    let time, currentTime = "00:00:00";
    let hour, min, sec;

    let audio = new Audio();

    audio.src = "static/timer.mp3";
    audio.setAttribute("loop", true);

    let clocks = loadTime();
    let alertVisible = false;

    const checkTimeAndAlert = (hour, min) => {
        const find = clocks.find((item) => item[0] === hour && item[1] === min);

        if (find) {
            audio.play();
            alertVisible = true;
        }
    };

    const cancelAlert = () => {
        audio.pause();
        audio.currentTime = 0;
        alertVisible = false;
    }

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

            // 只有秒是 0 的时候才判断是否需要响铃
            if (time[3] === 0) {
                checkTimeAndAlert(time[1], time[2]);
            }
        }, 1000);

        return () => clearInterval(interval);
    });

    let drawerVisible = false;

    const setVisible = () => {
        drawerVisible = !drawerVisible;
    }

    const onConfirm = (hour, min) => {
        clocks = [...clocks, [hour, min]];
        saveTime(clocks);
        setVisible();
    }
</script>

<div class={clsn("tool-notice", alertVisible && "active")}>
    闹钟时间已到！<button on:click={cancelAlert}>取消</button>
</div>
<div class="tool-content">
    <div class="tool-heading">
        <div class="tool-clock">
            <div class="clock-min" style="transform: rotate({min}deg)"></div>
            <div class="clock-hour" style="transform: rotate({hour}deg)"></div>
            <div class="clock-sec" style="transform: rotate({sec}deg)"></div>
        </div>
        <time>{currentTime}</time>
    </div>
    <ul class="tool-reconds">
        {#each clocks as item}
            <li>{`${zeroFill(item[0])}:${zeroFill(item[1])}`}</li>
        {/each}
    </ul>
</div>
<div class="tool-foot">
    <button on:click={setVisible}>添加</button>
</div>
<AddClock visible={drawerVisible} onConfirm={onConfirm} onClose={setVisible} />

<style>
    .tool-content {
        overflow: auto;
    }

    .tool-reconds {
        flex: none;
        height: calc(100% - 6.6em);
    }
</style>
