<script>
	import Clock from "./Clock.svelte";
	import Timer from "./Timer.svelte";
	import StopWatch from "./StopWatch.svelte";
	import Rand from "./Rand.svelte";

	let currentTab = 0, tabs = ["时钟", "倒计时", "秒表", "随机数"];

	function setTab(id){
		currentTab = id;
	}
</script>

<main class="kico-tools">
	<div class="tool-head">
		{#each tabs as tab, index}
			<span class={currentTab === index ? "active" : ""} on:click={() => setTab(index)}>{tab}</span>
		{/each}
	</div>
	{#if currentTab === 0}
		<Clock />
	{:else if currentTab === 1}
		<Timer />
	{:else if currentTab === 2}
		<StopWatch />
	{:else if currentTab === 3}
		<Rand />
	{/if}
</main>

<style>

</style>