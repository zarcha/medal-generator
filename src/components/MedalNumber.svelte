<script>
    let { value = $bindable() } = $props();
    let valid = $state(true);

    function inputValidation(){
        valid = value <= 255 && value >= 0;
    }
</script>

<div>
    <input type="number" class="form-control {valid ? '' : 'is-invalid'}" bind:value={value} oninput={inputValidation}>
</div>

<style>
    div {
        width: 300px;
        margin-left: auto;
        margin-right: auto;
        margin-bottom: 10px;
    }
</style>