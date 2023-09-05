<script>
    import { onMount } from "svelte";
    import { clsn } from "../../Tool.svelte";

    export let className = "";
    export let title = "标题";
    export let visible = false;
    export let onClose = () => {};

    const onKeyUp = (ev) => {
        ev.key === "Escape" && onClose();
    };
</script>

<svelte:body on:keyup={onKeyUp} />
<div class={clsn("tool-drawer", visible && "active", className)}>
    <header>
        <h1 class="title">{title}</h1>
        <button class="close" on:click={onClose}>×</button>
    </header>
    <main>
        <slot name="content">这是一个抽屉，添加一个子元素使它生效</slot>
    </main>
    {#if $$slots.footer}
        <footer class="tool-foot">
            <slot name="footer"></slot>
            <button on:click={onClose}>取消</button>
        </footer>
    {/if}
</div>

<style>
    .tool-drawer {
        left: 0;
        right: 0;
        bottom: 0;
        height: 80%;
        position: absolute;
        background-color: #fff;
        transform: translateY(110%);
        transition: transform .3s;
        box-shadow: 0 1px 5px 1px rgba(0, 0, 0, .1), 0 0 5px rgba(0, 0, 0, .1);
    }

    .tool-drawer.active {
        transform: translateY(0);
        outline: rgba(0, 0, 0, .3) 100em solid;
    }

    .tool-drawer header {
        padding: 1em;
    }

    .tool-drawer main {
        padding: 1em;
    }

    .tool-drawer .title {
        font-size: 1em;
    }

    .tool-drawer .close {
        top: 1em;
        right: 1em;
        width: 1em;
        cursor: pointer;
        position: absolute;
        text-align: center;
    }
</style>
