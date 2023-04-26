<script lang="ts">
	import { onMount } from 'svelte';
	import { GLTFLoader, type GLTF } from 'three/examples/jsm/loaders/GLTFLoader.js';
	import {
		Scene,
		WebGLRenderer,
		PerspectiveCamera,
		SpotLight,
		PointLight,
		DirectionalLight
	} from 'three';
	import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';

	onMount(() => {
		const sizes = {
			width: window.innerWidth,
			height: window.innerHeight
		};

		const renderer = new WebGLRenderer({
			antialias: true
		});
		renderer.setSize(window.innerWidth, window.innerHeight);
		renderer.domElement.id = 'background';
		document.body.appendChild(renderer.domElement);

		const scene = new Scene();

		const camera = new PerspectiveCamera(35, sizes.width / sizes.height, 0.1);

		const controls = new OrbitControls(camera, renderer.domElement);

		const loader = new GLTFLoader();

		const light = new DirectionalLight(0xffffff, 2);
		light.position.set(0, 5, 5);
		//const light = new PointLight(0xffffff, 2);
		//const light = new SpotLight(0xffffff, 2);
		//	light.position.set(5, 5, 5);
		scene.add(light);
		camera.position.x = -0.8175106127579147;
		camera.position.y = -0.8946030791556319;
		camera.position.z = 0.35487541991391613;
		controls.update();

		//camera.position.x = 0;
		//camera.position.y = 0.5;
		//camera.position.z = 1.2;

		let gltf: GLTF;
		loader.load('./planet.glb', function (loadedGltf) {
			gltf = loadedGltf;
			scene.add(gltf.scene);
			//gltf.scene.rotateY(0.3);
			animate();
			renderer.render(scene, camera);
		});

		function animate() {
			requestAnimationFrame(animate);

			controls.update();
			console.log(camera.position);

			renderer.render(scene, camera);
		}
	});
</script>

<svelte:head>
	<title>Aching Traveler</title>
	<meta name="description" content="Aching Traveler" />
</svelte:head>

<style>
	#background {
		position: fixed;
		height: 100vh;
		width: 100vw;
		background-color: rgb(0, 0, 0);
	}

	:global(body) {
		margin: 0;
		padding: 0;
	}
</style>
