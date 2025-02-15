<script>
    import {onMount} from "svelte";

    let { generating = $bindable() } = $props();

    onMount(() => {
        setTimeout(() => {
            generating = false;
        }, 2000)
    });
</script>

<div class="progress margin-bottom">
    <div class="progress-bar progress-bar-striped progress-bar-animated" style="width:100%">
        Generating...
    </div>
</div>

<style>
    .margin-bottom {
        margin-bottom: 20px;
    }
</style>