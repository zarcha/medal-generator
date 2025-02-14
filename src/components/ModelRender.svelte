<script>
    import * as THREE from 'three';
    import { STLLoader } from 'three/examples/jsm/loaders/STLLoader';
    import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';

    // Create Scene, Camera, and Renderer
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(300, 300);
    document.body.appendChild(renderer.domElement);

    // Load STL File
    const loader = new STLLoader();
    loader.load('medal.stl', function (geometry) {
        const material = new THREE.MeshStandardMaterial({ color: 0x0077ff, metalness: 0.5, roughness: 0.5 });
        const mesh = new THREE.Mesh(geometry, material);
        scene.add(mesh);

        // Center the model
        geometry.computeBoundingBox();
        const center = new THREE.Vector3();
        geometry.boundingBox.getCenter(center);
        mesh.position.sub(center);

        // Adjust camera
        camera.position.set(0, 0, 50);
    });

    // Add Lighting
    const light = new THREE.DirectionalLight(0xffffff, 1);
    light.position.set(5, 10, 5);
    scene.add(light);
    scene.add(new THREE.AmbientLight(0xffffff, 0.5));

    renderer.render(scene, camera);

</script>

<div>

</div>

<style>

</style>