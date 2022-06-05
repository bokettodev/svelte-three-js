<script lang="ts">
    import {
        BoxGeometry,
        Color,
        DirectionalLight,
        Mesh,
        MeshStandardMaterial,
        PCFSoftShadowMap,
        PerspectiveCamera,
        PlaneGeometry,
        Scene,
        WebGLRenderer
    } from 'three';
    import {onMount} from "svelte";

    let windowInnerWidth: number;
    let windowInnerHeight: number;
    let canvas: HTMLCanvasElement;

    let renderer: WebGLRenderer;

    const scene = new Scene();
    scene.background = new Color('blue')
    scene.rotateX(3);
    scene.rotateY(2.5);
    scene.rotateZ(2);

    const camera = new PerspectiveCamera(75, windowInnerWidth / windowInnerHeight, 0.1, 1000);
    camera.position.set(0, 0, 10)

    const cubeGeometry = new BoxGeometry(1, 1, 1);
    const cubeMaterial = new MeshStandardMaterial({color: 'yellow'});
    const cube = new Mesh(cubeGeometry, cubeMaterial);
    cube.castShadow = true;
    cube.position.z = 2;
    scene.add(cube);

    const light = new DirectionalLight;
    light.position.set(2.5, 5, 10)
    light.castShadow = true;
    scene.add(light);

    const planeGeometry = new PlaneGeometry(5, 5);
    const planeMaterial = new MeshStandardMaterial({color: 'green'})
    const plane = new Mesh(planeGeometry, planeMaterial);
    plane.receiveShadow = true;
    scene.add(plane);

    onMount(() => {
        renderer = new WebGLRenderer({antialias: true, canvas});
        renderer.shadowMap.enabled = true;
        renderer.shadowMap.type = PCFSoftShadowMap;

        renderer.setSize(windowInnerWidth, windowInnerHeight)
        camera.aspect = windowInnerWidth / windowInnerHeight;
        camera.updateProjectionMatrix();

        animate();
    })

    const animate = () => {
        requestAnimationFrame(animate);

        cube.rotation.x += 0.01;
        cube.rotation.y += 0.01;

        renderer.render(scene, camera);
    };
</script>

<svelte:window bind:innerWidth={windowInnerWidth} bind:innerHeight={windowInnerHeight}/>

<canvas bind:this={canvas}></canvas>