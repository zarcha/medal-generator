<script>
    let { value } = $props();
    let bytes = $state(['0','0','0','0','0','0','0','0']);

    function hex2bin(hex) {
        const tmp = '00000000' + parseInt(hex, 16).toString(2);
        return tmp.substring(tmp.length - 8);
    }

    function int2hex(value, isLittleEndian = false) {
        if (!Number.isInteger(value)) {
            throw new Error('Input must be an integer.');
        }

        if (value < 0) {
            throw new Error('This function does not support negative numbers for endianness conversion.');
        }

        let hex = value.toString(16).toUpperCase();

        if (hex.length % 2 !== 0) {
            hex = '0' + hex;
        }

        if (isLittleEndian) {
            const bytes = hex.match(/.{2}/g).reverse();
            hex = bytes.join('');
        }

        return hex;
    }

    $effect(() => {
        bytes = hex2bin(int2hex(value)).split('');
    });
</script>

<div class="center">
    <div class="hexagon">
        <div class="hole-container">
            {#each bytes.slice(0, 4) as bit}
            <div class="{bit === '0' ? 'hole-empty' : 'hole-filled'}"></div>
            {/each}
            <div class="spacer" />
            {#each bytes.slice(4, 8) as bit}
            <div class="{bit === '0' ? 'hole-empty' : 'hole-filled'}"></div>
            {/each}
        </div>
    </div>
</div>

<style>
    .hole-empty {
        height: 35px;
        width: 35px;
        background-color: #d3b40b;
        border-radius: 50%;
        display: inline-block;
        margin-left: 5px;
        margin-right: 5px;
        box-shadow: inset 0px 5px 10px rgba(0, 0, 0, 0.5);
    }

    .hole-filled {
        height: 35px;
        width: 35px;
        background-color: #d3b40b;
        border-radius: 50%;
        display: inline-block;
        margin-left: 5px;
        margin-right: 5px;
    }

    .spacer {
        height: 20px;
    }

    .hole-container {
        margin-left: 40px;
    }

    .hexagon {
        background-image: url("images/medal.png");
        background-size: 260px 300px;
        width: 260px;
        height: 300px;
        align-content: center;
        margin-left: auto;
        margin-right: auto;
        margin-bottom: 20px;
    }
</style>