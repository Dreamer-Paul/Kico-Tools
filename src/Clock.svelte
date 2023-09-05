<!-- 时钟 -->
<script>
    import { onMount } from "svelte";
    import { clsn, getTime, loadTime, saveTime, zeroFill } from "./Tool.svelte";
    import AddClock from "./components/clock/AddClock.svelte";

    // 展示内容
    let [displayTime, hour, min, sec] = getTime();
    let hourDeg = (hour * 30) + (min / 2);
    let minDeg = (min * 6) + (sec / 10);
    let secDeg = sec * 6;

    // 闹钟
    let clocks = loadTime();

    // 响铃
    const audio = new Audio("static/timer.mp3");
    audio.setAttribute("loop", true);

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
        const interval = setInterval(() => {
            const [newDisplayTime, hour, min, sec] = getTime();

            displayTime = newDisplayTime;
            hourDeg = (hour * 30) + (min / 2);
            minDeg = (min * 6) + (sec / 10);
            secDeg = sec * 6;

            // 只有秒是 0 的时候才判断是否需要响铃
            if (sec === 0) {
                checkTimeAndAlert(hour, min);
            }
        }, 1000);

        return () => clearInterval(interval);
    });

    // 增加闹钟
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
            <div class="clock-min" style="transform: rotate({minDeg}deg)"></div>
            <div class="clock-hour" style="transform: rotate({hourDeg}deg)"></div>
            <div class="clock-sec" style="transform: rotate({secDeg}deg)"></div>
        </div>
        <time>{displayTime}</time>
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
