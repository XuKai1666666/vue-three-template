<script setup lang="ts">
import { onMounted } from 'vue'
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 1000);
const renderer = new THREE.WebGLRenderer({ antialias: true });
const controls = new OrbitControls(camera, renderer.domElement);
const grid = new THREE.GridHelper(40, 20, 0x000000, 0x000000);
const light1 = new THREE.AmbientLight(0x333333);
const light2 = new THREE.SpotLight(0x888888);
const light3 = new THREE.HemisphereLight(0xffffff, 0x444444, 0.6);

// 加载模型
function iniLoad() {
  const loader = new THREE.ObjectLoader();

  loader.load(
    // 资源的URL
    "src/assets/uh-60-blackhawk-helicopter-threejs/uh-60-blackhawk-helicopter.json",

    // onLoad回调
    // Here the loaded data is assumed to be an object
    function (obj) {
      // Add the loaded object to the scene
      scene.add(obj);
      renderer.setSize(100, 100)
		renderer.render(scene, camera)
    },

    // onProgress回调
    function (xhr) {
      console.log((xhr.loaded / xhr.total * 100) + '% loaded');
    },

    // onError回调
    function (err) {
      console.error('An error happened');
    }
  );
}

//场景
function iniScene() {

  camera.position.set(-10, 20, 20);
  camera.lookAt(scene.position);
  renderer.setClearColor(0x333333);
  renderer.shadowMap.enabled = true;

  renderer.setSize(window.innerWidth, window.innerHeight);
  scene.add(new THREE.AxesHelper(4));
}
//灯光
function iniLight() {

  scene.add(light1);


  light2.position.set(0, 40, 30);
  light2.castShadow = true;
  light2.shadow.mapSize.height = 4096;
  light2.shadow.mapSize.width = 4096;
  scene.add(light2);

  light3.position.set(0, 200, 0);
  scene.add(light3);
}
//地面 和 辅助网格
// function iniPlane() {
//   // var planeGeo = new THREE.PlaneGeometry(40, 40);
//   var planeMat = new THREE.MeshPhongMaterial({ color: 0x999999 });
//   // var plane = new THREE.Mesh(planeGeo, planeMat);
//   plane.receiveShadow = true;
//   plane.position.y = -0.01;
//   plane.rotation.x = -0.5 * Math.PI;
//   scene.add(plane);


//   // grid.material.transparent = true;
//   // grid.material.opacity = 0.3;
//   scene.add(grid);
// }
//相机轨道控制器
function orbitControls() {

  //自转
  controls.autoRotate = true;
  controls.autoRotateSpeed = 0.02;
  //阻尼 阻尼系数
  controls.enableDamping = true;
  controls.dampingFactor = 0.4;
  //缩放
  controls.enableZoom = true;
  controls.minDistance = 5;
  controls.maxDistance = 100;
  //右键拖拽
  controls.enablePan = true;
}
//改变窗口大小
function windowResize() {
  window.addEventListener('resize', onWindowResize, false);
  function onWindowResize() {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  }
}


function drawScene() {
  iniScene();
  iniLight();
  orbitControls();
  windowResize();
  // iniPlane();
  iniLoad();
}
drawScene();

onMounted(() => {
  //将渲染器绘制出的canvas添加到页面中
  document.getElementById('demo')?.appendChild(renderer.domElement)
})
</script>

<template>
<div id="demo"></div>
</template>

<style scoped>

</style>
