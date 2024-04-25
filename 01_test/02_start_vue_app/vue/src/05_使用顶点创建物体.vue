<!--
 * @Author: wangcongying
 * @Date: 2024-04-25 14:17:44
 * @LastEditTime: 2024-04-25 17:48:00
 * @Description:
-->
<script setup>
import * as THREE from "three";
//控制器 可以手动旋转
import { OrbitControls } from "three/addons/controls/OrbitControls.js";

// 导入lil.gui
import { GUI } from "three/addons/libs/lil-gui.module.min.js";

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

// 创建几何体三角形
const geometry = new THREE.BufferGeometry();
// 创建顶点数据 顶点是有顺序的 逆时针为正面 顺时针为反面
// const vertices = new Float32Array([
//   -1.0,
//   -1.0,
//   0.0, //顶点1
//   1.0,
//   -1.0,
//   0.0, //顶点2
//   1.0,
//   1.0,
//   0.0, //顶点3

//   1.0,
//   1.0,
//   0.0,
//   -1.0,
//   1.0,
//   0.0,
//   -1.0,
//   -1.0,
//   0.0,
// ]);

// // 创建顶点属性
// geometry.setAttribute("position", new THREE.BufferAttribute(vertices, 3));

// 使用索引绘制
const vertices = new Float32Array([
  -1.0,
  -1.0,
  0.0, //顶点1
  1.0,
  -1.0,
  0.0, //顶点2
  1.0,
  1.0,
  0.0, //顶点3
  -1.0,
  1.0,
  0.0,
]);

geometry.setAttribute("position", new THREE.BufferAttribute(vertices, 3));
// 创建索引
const indices = new Uint16Array([0, 1, 2, 2, 3, 0]); // 顶点数三个为一组 0,1,2  2,3,0
// 创建索引属性
// 通过索引可以共用顶点 若不使用索引则顶点数为6
geometry.setIndex(new THREE.BufferAttribute(indices, 1));

// 设置2个顶点组,形成2个材质
geometry.addGroup(0, 3, 0); // 0索引开始 添加3个顶点,用的第一个材质
geometry.addGroup(3, 3, 1); // 3索引开始(2) 添加3个顶点 使用第二个材质
// 创建材质
const material = new THREE.MeshBasicMaterial({
  color: 0x00ff00,
  // side: THREE.DoubleSide, // 正反面都可以看到
  wireframe: true,
});
const material1 = new THREE.MeshBasicMaterial({
  color: 0xff0000,
});
const cubeMaterial = new THREE.MeshBasicMaterial({
  color: 0x0000ff,
});
const cubeMaterial1 = new THREE.MeshBasicMaterial({
  color: 0x00ffff,
});
const cubeMaterial2 = new THREE.MeshBasicMaterial({
  color: 0xffff00,
});
const cubeMaterial3 = new THREE.MeshBasicMaterial({
  color: 0xff00ff,
});
const cubeMaterial4 = new THREE.MeshBasicMaterial({
  color: 0xffffff,
});
const cubeMaterial5 = new THREE.MeshBasicMaterial({
  color: 0xeeee90,
});

const cube = new THREE.Mesh(new THREE.BoxGeometry(1, 1, 1), [
  cubeMaterial,
  cubeMaterial1,
  cubeMaterial2,
  cubeMaterial3,
  cubeMaterial4,
  cubeMaterial5,
]);
cube.position.x = 2;

scene.add(cube);

// 创建网格
const plane = new THREE.Mesh(geometry, [material, material1]);
// 添加场景
scene.add(plane);
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

// 监听窗口变化
window.addEventListener("resize", () => {
  // 更新相机
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  // 更新渲染器
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.setPixelRatio(window.devicePixelRatio);
});

// 创建GUI
const gui = new GUI();
</script>

<template>
  <div></div>
</template>

<style>
</style>
