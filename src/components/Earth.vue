<template>
  <div></div>
</template>

<script>
import * as THREE from "three";
import { RGBELoader } from "three/examples/jsm/loaders/RGBELoader";

import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
export default {
  name: "EarthExample",
  setup() {
    // 初始化场景
    const scene = new THREE.Scene();

    // 初始化相机
    const camera = new THREE.PerspectiveCamera(
      45,
      window.innerWidth / window.innerHeight,
      1,
      1000
    );
    camera.position.set(10, 10, 10);
    scene.add(camera);

    // const texture = new THREE.TextureLoader().load(
    //   require("../assets/earth_bg.png")
    // );
    const texture = new RGBELoader().load("limpopo_golf_course_4k.hdr");

    const geometry = new THREE.SphereGeometry(2, 30, 30);
    const material = new THREE.MeshBasicMaterial({
      map: texture,
      side: THREE.DoubleSide,
    });
    const mesh = new THREE.Mesh(geometry, material);
    scene.add(mesh);

    // 渲染器
    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    // renderer.setClearColor(0xeeeeee);
    renderer.outputEncoding = THREE.sRGBEncoding;
    document.body.appendChild(renderer.domElement);

    new OrbitControls(camera, renderer.domElement);
    // const gridHelper = new THREE.GridHelper(20);
    // scene.add(gridHelper);

    // scene.fog = new THREE.Fog(0xffffff, 10, 35);

    // function addLight() {
    //   const light = new THREE.DirectionalLight(0xffffff, 1);
    //   light.position.set(0, 0, 3);
    //   const lightHelper = new THREE.DirectionalLightHelper(light, 1);
    //   scene.add(light);
    //   scene.add(lightHelper);

    //   const light2 = new THREE.DirectionalLight(0xffffff, 1);
    //   light2.position.set(0, 0, -3);
    //   const lightHelper2 = new THREE.DirectionalLightHelper(light2, 1);
    //   scene.add(light2);
    //   scene.add(lightHelper2);

    //   const light3 = new THREE.DirectionalLight(0xffffff, 1);
    //   light3.position.set(0, 3, 0);
    //   const lightHelper3 = new THREE.DirectionalLightHelper(light3, 1);
    //   scene.add(light3);
    //   scene.add(lightHelper3);
    // }
    // addLight();

    animate();
    function animate() {
      // mesh.rotation.y += 0.01;
      renderer.render(scene, camera);
      requestAnimationFrame(animate);
    }
  },
};
</script>

<style>
</style>