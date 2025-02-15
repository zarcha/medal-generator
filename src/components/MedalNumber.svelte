<script>
    let { value = $bindable(), generating = $bindable() } = $props();
    let valid = $state(true);
    let tmpValue = $state()

    function inputValidation(){
        valid = tmpValue < 255 && tmpValue > 0;
    }

    function generate(){
        if(value){
            generating = true;
            value = tmpValue;
        }
    }
</script>

<div>
    <div class="input-group mb-3">
        <input type="number" class="form-control {valid ? '' : 'is-invalid'}" bind:value={tmpValue} oninput={inputValidation} placeholder="Medal Number 1-254">
        <button class="btn btn-warning" onclick={generate}>Generate</button>
    </div>
</div>

<style>
    div {
        width: 300px;
        margin-left: auto;
        margin-right: auto;
        margin-bottom: 20px;
    }
</style>