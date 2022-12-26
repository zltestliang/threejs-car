<template>
  <div>
    <button type="button" @click="onOpen">打开车门</button>
    <button type="button" @click="onClose">关闭车门</button>
    <button type="button" @click="onChangeColor">更改颜色</button>
  </div>
</template>

<script>
import * as THREE from "three";
import gsap from "gsap";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
import { DRACOLoader } from "three/examples/jsm/loaders/DRACOLoader";

import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

var Mt_ABS_Black_Mat = new THREE.MeshStandardMaterial({
  color: 0x000000,
  roughness: 0.6,
  metalness: 0.5,
});
var Mt_WindScreens = new THREE.MeshStandardMaterial({
  color: 0x000000,
  roughness: 0.0,
  metalness: 0.0,
  transparent: true,
  opacity: 0.7,
});
var bodyMaterial = new THREE.MeshStandardMaterial({
  color: 0xd24a57,
  roughness: 0,
  metalness: 0,
});

export default {
  name: "EarthExample",
  setup() {
    let lfDoor;
    let lbDoor;
    let rfDoor;
    let rbDoor;
    let sunproof;
    let backDoor;
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

    const dracoLoader = new DRACOLoader();
    dracoLoader.setDecoderPath("draco/gltf/");

    const loader = new GLTFLoader();
    loader.setDRACOLoader(dracoLoader);
    loader.load("model/Lynkco09_EXT_d.glb", function (gltf) {
      gltf.scene.traverse((child) => {
        if (child.type === "Mesh") {
          if (child.name.indexOf("EXT") > -1) {
            switch (child.name) {
              case "EXT_06":
              case "EXT_07":
              case "EXT_34":
                child.material = Mt_WindScreens;
                break;
              case "EXT_51":
              case "EXT_03":
              case "EXT_01":
                child.material = bodyMaterial;
                break;
              default:
                child.material = Mt_ABS_Black_Mat;
            }
          }
        }
      });
      scene.add(gltf.scene);
    });

    loader.load("model/Lynkco09_INT_d.glb", function (gltf) {
      console.log(gltf);
      gltf.scene.traverse((child) => {
        if (child.type === "Mesh") {
          if (child.name.indexOf("INT") > -1) {
            switch (child.name) {
              case "EXT_06":
              case "EXT_07":
              case "EXT_34":
                child.material = Mt_WindScreens;
                break;
              default:
                child.material = Mt_ABS_Black_Mat;
            }
          }
        }
      });
      scene.add(gltf.scene);
    });

    loader.load("model/Lynkco09_LBDoor_d.glb", function (gltf) {
      console.log(gltf);
      gltf.scene.traverse((child) => {
        if (child.type === "Mesh") {
          const name = child.name.slice(1);
          switch (name) {
            case "BDoor_03":
            case "FDoor_08":
              child.material = Mt_WindScreens;
              break;
            case "BDoor_05":
            case "FDoor_01":
              child.material = bodyMaterial;
              break;
            default:
              child.material = Mt_ABS_Black_Mat;
          }
        }
      });

      lbDoor = new THREE.Group();
      lbDoor.add(gltf.scene);
      lbDoor.position.set(0.77, 0, -0.1);
      gltf.scene.position.set(-0.77, 0, 0.1);
      scene.add(lbDoor);
    });

    loader.load("model/Lynkco09_LFDoor_d.glb", function (gltf) {
      gltf.scene.traverse((child) => {
        if (child.type === "Mesh") {
          const name = child.name.slice(1);
          switch (name) {
            case "BDoor_03":
            case "FDoor_08":
              child.material = Mt_WindScreens;
              break;
            case "BDoor_05":
            case "FDoor_01":
              child.material = bodyMaterial;
              break;
            default:
              child.material = Mt_ABS_Black_Mat;
          }
        }
      });

      lfDoor = new THREE.Group();
      lfDoor.add(gltf.scene);
      lfDoor.position.set(0.75, 0, 0.75);
      gltf.scene.position.set(-0.75, 0, -0.75);
      scene.add(lfDoor);
    });

    loader.load("model/Lynkco09_RBDoor_d.glb", function (gltf) {
      gltf.scene.traverse((child) => {
        if (child.type === "Mesh") {
          const name = child.name.slice(1);
          switch (name) {
            case "BDoor_03":
            case "FDoor_08":
              child.material = Mt_WindScreens;
              break;
            case "BDoor_05":
            case "FDoor_01":
              child.material = bodyMaterial;
              break;
            default:
              child.material = Mt_ABS_Black_Mat;
          }
        }
      });

      rbDoor = new THREE.Group();
      rbDoor.add(gltf.scene);
      rbDoor.position.set(-0.75, 0, -0.1);
      gltf.scene.position.set(0.75, 0, 0.1);
      scene.add(rbDoor);
    });

    loader.load("model/Lynkco09_RFDoor_d.glb", function (gltf) {
      gltf.scene.traverse((child) => {
        if (child.type === "Mesh") {
          const name = child.name.slice(1);
          switch (name) {
            case "BDoor_03":
            case "FDoor_08":
              child.material = Mt_WindScreens;
              break;
            case "BDoor_05":
            case "FDoor_01":
              child.material = bodyMaterial;
              break;
            default:
              child.material = Mt_ABS_Black_Mat;
          }
        }
      });

      rfDoor = new THREE.Group();
      rfDoor.add(gltf.scene);
      rfDoor.position.set(-0.75, 0, +0.73);
      gltf.scene.position.set(0.75, 0, -0.73);
      scene.add(rfDoor);
    });

    loader.load("model/Lynkco09_Sunproof_d.glb", function (gltf) {
      gltf.scene.traverse((child) => {
        if (child.type === "Mesh") {
          const name = child.name;
          if (name === "Sunproof_03") {
            sunproof = child;
          }
          switch (name) {
            case "Sunproof_01":
            case "Sunproof_03":
              child.material = Mt_WindScreens;
              break;
            default:
              child.material = Mt_ABS_Black_Mat;
          }
        }
      });

      scene.add(gltf.scene);
    });

    // 加载轮胎
    loader.load("model/Lynkco09_Tires_d.glb", function (gltf) {
      gltf.scene.traverse((child) => {
        if (child.type === "Mesh") {
          const name = child.name;
          switch (name) {
            case "Sunproof_01":
            case "Sunproof_03":
              child.material = Mt_WindScreens;
              break;
            default:
              child.material = Mt_ABS_Black_Mat;
          }
        }
      });
      scene.add(gltf.scene);
    });

    // // 加载后备箱
    loader.load("model/Lynkco09_Trunk_d.glb", function (gltf) {
      gltf.scene.traverse((child) => {
        if (child.type === "Mesh") {
          const name = child.name;
          switch (name) {
            case "Trunk_03":
            case "Trunk_04":
              child.material = Mt_WindScreens;
              break;
            case "Trunk_93":
              // 汽车车标
              child.material = new THREE.MeshStandardMaterial({
                color: 0x000000,
                metalness: 0.1,
                antialias: 0.4,
              });
              break;
            default:
              child.material = bodyMaterial;
          }
        }
      });

      backDoor = new THREE.Group();
      backDoor.add(gltf.scene);
      backDoor.position.set(0, 1.42, -1.44);
      gltf.scene.position.set(0, -1.42, 1.44);

      scene.add(backDoor);
    });

    // 渲染器
    const renderer = new THREE.WebGLRenderer({
      antialias: true,
      alpha: true,
    });
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.outputEncoding = THREE.sRGBEncoding;
    document.body.appendChild(renderer.domElement);

    new OrbitControls(camera, renderer.domElement);
    // const gridHelper = new THREE.GridHelper(20);
    // scene.add(gridHelper);

    // 添加灯光
    const light1 = new THREE.DirectionalLight(0xffffff, 0.2);
    light1.position.set(0, 0, 10);
    scene.add(light1);
    const light2 = new THREE.DirectionalLight(0xffffff, 0.2);
    light2.position.set(0, 0, -10);
    scene.add(light2);
    const light3 = new THREE.DirectionalLight(0xffffff, 0.2);
    light3.position.set(10, 0, 0);
    scene.add(light3);
    const light4 = new THREE.DirectionalLight(0xffffff, 0.2);
    light4.position.set(-10, 0, 0);
    scene.add(light4);

    const light5 = new THREE.DirectionalLight(0xffffff, 0.2);
    light5.position.set(0, 10, 0);
    scene.add(light5);
    const light6 = new THREE.DirectionalLight(0xffffff, 0.2);
    light6.position.set(5, 10, 0);
    scene.add(light6);
    const light7 = new THREE.DirectionalLight(0xffffff, 0.2);
    light7.position.set(0, 10, 5);
    scene.add(light7);
    const light8 = new THREE.DirectionalLight(0xffffff, 0.2);
    light8.position.set(0, 10, -5);
    scene.add(light8);
    const light9 = new THREE.DirectionalLight(0xffffff, 0.2);
    light9.position.set(-5, 10, 0);
    scene.add(light9);

    // scene.add(new THREE.AxesHelper(20));

    animate();
    function animate() {
      renderer.render(scene, camera);
      requestAnimationFrame(animate);
    }

    // window.addEventListener("click", function () {}, false);

    return {
      onClose() {
        gsap.to(lfDoor.rotation, {
          duration: 0.55,
          y: 0,
        });

        gsap.to(lbDoor.rotation, {
          duration: 0.55,
          y: 0,
        });

        gsap.to(rfDoor.rotation, {
          duration: 0.55,
          y: 0,
        });

        gsap.to(rbDoor.rotation, {
          duration: 0.55,
          y: 0,
        });

        gsap.to(sunproof.position, {
          duration: 3,
          y: 0,
          z: 0,
        });

        gsap.to(backDoor.rotation, {
          duration: 2,
          x: 0,
        });
      },
      onOpen() {
        gsap.to(lfDoor.rotation, {
          duration: 0.55,
          y: -Math.PI / 4,
        });

        gsap.to(lbDoor.rotation, {
          duration: 0.55,
          y: -Math.PI / 4,
        });

        gsap.to(rfDoor.rotation, {
          duration: 0.55,
          y: Math.PI / 4,
        });

        gsap.to(rbDoor.rotation, {
          duration: 0.55,
          y: Math.PI / 4,
        });

        gsap.to(sunproof.position, {
          duration: 3,
          y: 3,
          z: -50,
        });

        gsap.to(backDoor.rotation, {
          duration: 2,
          x: Math.PI / 2,
        });
      },
      onChangeColor() {
        console.log(bodyMaterial.color);
        bodyMaterial.color = new THREE.Color(
          1 * Math.random(),
          1 * Math.random(),
          1 * Math.random()
        );
      },
    };
  },
};
</script>

  <style>
</style>