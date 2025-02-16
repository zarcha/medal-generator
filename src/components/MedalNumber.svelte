<script>
    import axios from 'axios';
    import {onMount} from "svelte";

    let { value = $bindable(), generating = $bindable() } = $props();
    let valid = $state(true);
    let tmpValue = $state();
    let medals = $state([]);
    let matchingMedals = $state([]);

    function inputValidation(){
        valid = tmpValue > 0 && tmpValue <= 510 && (tmpValue < 255 || tmpValue > 256);
    }

    function generate(){
        if(valid){
            generating = true;
            value = tmpValue;
        }
    }

    async function getMedalList(){
        const res = await axios.get('medals.json');
        medals = res.data;
    }

    function getMedals(id){
        return medals.filter((medal) => {
           if(medal.id === id){
               return medal;
           }
        });
    }

    onMount(() => {
        getMedalList();
    });

    $effect(() => {
        if(medals.length > 0){
            matchingMedals = getMedals(tmpValue);
        }
    });
</script>

<div style="text-align: center">
    <div class="input-group mb-3">
        <input type="number" class="form-control {valid ? '' : 'is-invalid'}" bind:value={tmpValue} oninput={inputValidation} placeholder="1-254, 257-510">
        <button class="btn btn-warning" onclick={generate}>Generate</button>
    </div>
    {#if matchingMedals.length > 0}
    {#each  matchingMedals as medal}
    <p style="color: green">Name: {medal.name} - Stage: {medal.stage}</p>
    {/each}
    {:else if value && valid}
    <p style="color: orange">No Confirmed Medals</p>
    {/if}
</div>

<style>
    div {
        width: 300px;
        margin-left: auto;
        margin-right: auto;
        margin-bottom: 20px;
    }
</style>