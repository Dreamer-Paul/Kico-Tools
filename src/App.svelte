<script>
    import Clock from "./Clock.svelte";
    import Timer from "./Timer.svelte";
    import StopWatch from "./StopWatch.svelte";
    import Rand from "./Rand.svelte";

    let currentTab = 0;
    let tabs = ["时钟", "倒计时", "秒表", "随机数"];

    function setTab(id) {
        currentTab = id;
    }
</script>

<main class="kico-tools">
    <div class="tool-head">
        {#each tabs as tab, index}
            <button class={currentTab === index ? "active" : ""} on:click={() => setTab(index)}>{tab}</button>
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
<footer>
    <p>© {new Date().getFullYear()} By <a href="https://paul.ren" target="_blank">Dreamer-Paul</a>. Star this at <a href="https://github.com/Dreamer-Paul/Kico-Tools" target="_blank">GitHub</a></p>
</footer>

<style>
    footer{
        padding: .75em 0;
        text-align: center;
    }

    @media screen and (max-width: 768px) {
        footer {
            display: none;
        }
    }
    footer p{
        color: #666;
        font-size: .875em;
    }
    footer a{
        color: #3498db;
        text-decoration: none;
    }
</style>
