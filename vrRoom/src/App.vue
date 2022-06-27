<template>
  <div class="container" ref="containerRef"></div>
</template>

<script setup>
import * as THREE from "three";
import { ref, onMounted } from "vue";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import { RGBELoader } from "three/examples/jsm/loaders/RGBELoader";

const containerRef = ref();
const innerWidth = window.innerWidth;
const innerHeight = window.innerHeight;
// 新建场景
const scene = new THREE.Scene();

// // 添加长方体
// const boxGeometry = new THREE.BoxGeometry(10, 10, 10);

// // 贴图
// const imgList = ["4_l", "4_r", "4_u", "4_d", "4_b", "4_f"];
// let boxMaterials = [];
// imgList.forEach((item) => {
//   // 纹理加载
//   let texture = new THREE.TextureLoader().load(`./imgs/living/${item}.jpg`);
//   // 创建材质
//   if (["4_u", "4_d"].includes(item)) {
//     texture.rotation = Math.PI;
//     texture.center = new THREE.Vector2(0.5, 0.5);
//   }
//   boxMaterials.push(new THREE.MeshBasicMaterial({ map: texture }));
// });
// const cub = new THREE.Mesh(boxGeometry, boxMaterials);
// cub.geometry.scale(1, 1, -1);
// scene.add(cub);

// 添加球体

const sphereGeometry = new THREE.SphereBufferGeometry(5, 32, 32);
const loader = new RGBELoader();
loader.load("./imgs/hdr/Living.hdr", (texture) => {
  const material = new THREE.MeshBasicMaterial({ map: texture });
  const sphere = new THREE.Mesh(sphereGeometry, material);
  sphere.geometry.scale(1, 1, -1);
  scene.add(sphere);
});

//初始化相机
const camera = new THREE.PerspectiveCamera(
  75,
  innerWidth / innerHeight,
  0.1,
  1000
);
camera.position.z = 5;
// 初始化渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(innerWidth, innerHeight);

//渲染函数
const render = () => {
  renderer.render(scene, camera);
  requestAnimationFrame(render); //请求动画帧
};

onMounted(() => {
  containerRef.value.appendChild(renderer.domElement);
  const controls = new OrbitControls(camera, containerRef.value);
  controls.enableDamping = true; //添加阻尼效果
  render();
});
</script>
<style>
* {
  margin: 0;
  padding: 0;
}
.container {
  height: 100vh;
  width: 100vw;
  background-color: #f0f0f0;
}
</style>
