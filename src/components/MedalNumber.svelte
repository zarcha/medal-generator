<script>
    let { value = $bindable() } = $props();
    let tmpNumber = $state(0);
</script>

<div>
    <div class="input-group mb-3">
        <input type="number" class="form-control" bind:value={tmpNumber}>
        <button class="btn btn-warning" onclick={() => {value = tmpNumber}}>Generate</button>
    </div>
</div>

<style>
    div {
        width: 300px;
        margin-left: auto;
        margin-right: auto;
        margin-bottom: 10px;
    }
</style>