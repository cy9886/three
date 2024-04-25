<!--
 * @Author: wangcongying
 * @Date: 2024-04-25 14:17:44
 * @LastEditTime: 2024-04-25 16:28:14
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

// 创建几何体
const geometry = new THREE.BoxGeometry(1, 1, 1);
// 创建材质
const parentCubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 });
// 父元素改为线性元素
parentCubeMaterial.wireframe = true;
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
// 创建网格
let parentCube = new THREE.Mesh(geometry, parentCubeMaterial);

const cube = new THREE.Mesh(geometry, material);
parentCube.add(cube); // 父元素中添加子元素
// 设置物体位置(局部坐标)
// cube.position.x = 2;
cube.position.set(3, 0, 0);
parentCube.position.set(-3, 0, 0); // 展示出来的会是 cube在parentCube距离3的位置 因为cube是针对于parentCube
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

// 监听窗口变化
window.addEventListener("resize", () => {
  // 更新相机
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  // 更新渲染器
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.setPixelRatio(window.devicePixelRatio);
});

// // 监听按钮进行全屏展示
// const fullScreenBtn = document.createElement("button");
// fullScreenBtn.innerText = "全屏展示";
// fullScreenBtn.style.position = "absolute";
// fullScreenBtn.style.top = "10px";
// fullScreenBtn.style.left = "10px";
// fullScreenBtn.onclick = function () {
//   renderer.domElement.requestFullscreen();
// }
// document.body.appendChild(fullScreenBtn);
let eventObj = {
  fullScreen: function () {
    renderer.domElement.requestFullscreen();
  },
};

// 创建GUI
const gui = new GUI();
// 添加按钮
gui.add(eventObj, "fullScreen");

// 控制物体位置
let folder = gui.addFolder("立方体位置");
folder
  .add(cube.position, "x")
  .min(-10)
  .max(10)
  .step(1)
  .name("立方体x轴位置")
  .onChange((val) => {
    console.log(val, "x轴移动");
  });
folder
  .add(cube.position, "y")
  .min(-10)
  .max(10)
  .step(1)
  .name("立方体y轴位置")
  .onFinishChange((val) => {
    console.log(val, "y轴移动鼠标移开事件");
  });
folder.add(cube.position, "z").min(-10).max(10).step(1).name("立方体z轴位置");
// gui.add(cube.position, "x", -5, 5).name("位置x")
gui.add(parentCubeMaterial, "wireframe").name("父元素线性模式");
let colorParam = {
  cubeColor: "#00ff00",
}
gui.addColor(colorParam, "cubeColor").onChange((val) => {
  cube.material.color.set(val);
});
</script>

<template>
  <div></div>
</template>

<style>
</style>
