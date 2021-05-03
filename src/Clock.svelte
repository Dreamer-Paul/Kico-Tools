<!-- 时钟 -->
<script>
    import { onMount } from "svelte";
    import { getTime } from "./Tool.svelte";

    let currentTime = "00:00:00";

    onMount(() => {
        currentTime = getTime();

		const interval = setInterval(() => {
			currentTime = getTime();
		}, 1000);

		return () => clearInterval(interval);
	});
</script>

<div class="tool-content">
    <div class="tool-notice"></div>
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
