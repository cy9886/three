<!--
 * @Author: wangcongying
 * @Date: 2024-04-25 14:17:44
 * @LastEditTime: 2024-04-25 15:11:13
 * @Description:
-->
<script setup>
import * as THREE from "three";
//控制器 可以手动旋转
import { OrbitControls } from "three/addons/controls/OrbitControls.js";

//创建场景
const scene = new THREE.Scene();

// 创建相机
const camera = new THREE.PerspectiveCamera(
  75, // 视角 视角越大看到的越多
  window.innerWidth / window.innerHeight, // 宽高比
  0.1, // 近平面
  1000 // 远平面
);

// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);


// 创建几何体
const geometry = new THREE.BoxGeometry(1, 1, 1);
// 创建材质
const parentCubeMaterial = new THREE.MeshBasicMaterial({ color:  0xffff00});
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
// 创建网格
let parentCube = new THREE.Mesh(geometry, parentCubeMaterial);

const cube = new THREE.Mesh(geometry, material);
parentCube.add(cube); // 父元素中添加子元素
// 设置物体位置(局部坐标)
// cube.position.x = 2;
cube.position.set(3, 0, 0)
parentCube.position.set(-3, 0, 0) // 展示出来的会是 cube在parentCube距离3的位置 因为cube是针对于parentCube
// 设置立方体的放大(局部放大 针对于父元素,若无父元素,则是针对于世界坐标)
// cube.scale.set(2, 2, 2);
// 立方体旋转(Math.PI / 4为45度)
cube.rotation.set(Math.PI / 4, 0, 0);

// 添加到场景中
scene.add(parentCube);
// 设置相机位置
camera.position.z = 5;
camera.position.x = 2;
camera.position.y = 2;
camera.lookAt(0, 0, 0);

// 添加世界坐标辅助器
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);

// 添加控制器
const controls = new OrbitControls(camera, renderer.domElement);
// 设置带阻尼的惯性
controls.enableDamping = true;
// 设置阻尼系数
controls.dampingFactor = 0.05;
// 设置自动旋转
// controls.autoRotate = true;
// 渲染函数 自动旋转
function animate() {
  controls.update();
  requestAnimationFrame(animate);
  // cube.rotation.x += 0.01;
  // cube.rotation.y += 0.01;
  // 渲染
  renderer.render(scene, camera);
}
animate();
</script>

<template>
  <div></div>
</template>

<style>
</style>
