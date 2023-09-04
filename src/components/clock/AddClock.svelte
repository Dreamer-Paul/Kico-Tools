<script>
    import { zeroFill } from "../../Tool.svelte";
    import Drawer from "../ui/Drawer.svelte";

    export let visible = false;
    export let onClose = () => {};
    export let onConfirm = () => {};

    const hours = (() => {
        let num = 0;
        const result = [];

        while (num < 24) {
            result.push(num);
            num++;
        }

        return result;
    })();

    const min = (() => {
        let num = 0;
        const result = [];

        while (num < 60) {
            result.push(num);
            num += 5;
        }

        return result;
    })();

    let inputHour = 0;
    let inputMin = 0;
</script>

<Drawer title="添加闹钟" className="add-clock" visible={visible} onClose={onClose}>
    <div slot="content" class="input-group">
        <select class="input" bind:value={inputHour}>
            {#each hours as item}
                <option value={item}>{zeroFill(item)}</option>
            {/each}
        </select>
        <span>时</span>
        <select class="input" bind:value={inputMin}>
            {#each min as item}
                <option value={item}>{zeroFill(item)}</option>
            {/each}
        </select>
        <span>分</span>
    </div>
    <button slot="footer" on:click={onConfirm(inputHour, inputMin)}>添加闹钟</button>
</Drawer>

<style>
    :global(.tool-drawer.add-clock) {
        display: flex;
        flex-direction: column;
    }

    :global(.tool-drawer.add-clock main) {
        flex: 1;
    }

    .input {
        height: 1.5em;
    }

    .input-group span {
        font-size: 1rem;
    }
</style>
