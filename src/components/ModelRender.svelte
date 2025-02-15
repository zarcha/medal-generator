<script>
    import {delay, hex2bin, int2hex} from "../lib/util.js";
    import * as THREE from 'three';
    import { STLLoader } from 'three/examples/jsm/loaders/STLLoader';
    import { STLExporter} from "three/addons";

    let { value } = $props();
    let generating = $state(false);
    let scene;
    let camera;
    let renderer;
    let medalMesh;
    const globalMaterial = new THREE.MeshStandardMaterial({ color: 0xd3b40b, metalness: 0.5, roughness: 0.5 });

    function setup(){
        scene = new THREE.Scene();
        camera = new THREE.PerspectiveCamera(45, 300 / 300, .1, 1000);
        renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
        renderer.setClearColor(0x000000, 0);
        const canvas = document.getElementById('canvas');
        renderer.setSize(canvas.offsetWidth, canvas.offsetHeight);
        canvas.innerHTML = '';
        canvas.appendChild(renderer.domElement);
        camera.rotateY(Math.PI);
        camera.position.set(0, 0, -37);

        const light = new THREE.DirectionalLight(0xffffff, 1);
        light.position.set(5, 10, -30);
        scene.add(light);
    }

    function loadMedalSTL(){
        return new Promise((resolve) => {
            const loader = new STLLoader();
            loader.load('medal.stl', function (geometry) {
                const material = globalMaterial;
                geometry.center();
                medalMesh = new THREE.Mesh(geometry, material);
                resolve();
            });
        });
    }

    function fillHole(hole){
        let posY = hole < 4 ? 5 : -4;
        hole = hole > 3 ? hole - 4 : hole;
        let posX = (4.5 * hole) - 6.75;
        const geometry = new THREE.CylinderGeometry(1.3, 1.3, 1.25, 32);
        const material = globalMaterial;
        geometry.rotateX(90 * Math.PI / 180)

        const fillMesh = new THREE.Mesh(geometry, material);
        fillMesh.position.set(posX, posY, -0.38);
        fillMesh.updateMatrix();

        scene.add(fillMesh)
    }

    async function exportSceneToSTL() {
        const exporter = new STLExporter();
        const stlData = exporter.parse(scene);

        const fileHandle = await window.showSaveFilePicker({
            suggestedName: `Medal_${value}.stl`,
            types: [
                {
                    description: 'Medal Save',
                    accept: { 'application/octet-stream': ['.stl'] }
                }
            ]
        });

        const writableStream = await fileHandle.createWritable();
        await writableStream.write(stlData);
        await writableStream.close();
    }

    async function generateMedal(){
        if(value <= 255 && value >= 0){
            generating = true;
            setup();
            await loadMedalSTL();
            const tmpBitArray = hex2bin(int2hex(value)).split('');
            const bitArray = tmpBitArray.slice(0, 4).reverse().concat(tmpBitArray.slice(4, 8).reverse());

            for(let i = 0; i < bitArray.length; i++) {
                if (bitArray[i] === '1') {
                    fillHole(i);
                }
            }

            scene.add(medalMesh);
            renderer.render(scene, camera);
            generating = false;
        }
    }
</script>

<div class="medal-container">

    <div id="canvas">
        {#if generating}
            <div>Generating Medal...</div>
        {/if}
    </div>
    <button class="btn btn-warning" onclick={exportSceneToSTL}>Save Medal</button>
    <button class="btn btn-warning" onclick={generateMedal}>Generate Medal</button>
</div>

<style>
    .medal-container {
        text-align: center;
        margin-bottom: 10px;
        border-radius: 50px;
    }

    #canvas {
        width: 300px;
        height: 300px;
        margin: 10px auto;
    }
</style>