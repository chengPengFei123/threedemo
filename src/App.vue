<template>
  <div ref="container" class="container">

  </div>
</template>

<script setup>
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
// import { RGBELoader } from 'three/examples/jsm/loaders/RGBELoader'
import { ref, onMounted } from 'vue'
// 初始化场景
const scene = new THREE.Scene();
// 初始化相机
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);
// 设置相机位置
camera.position.z = 5;
// 初始化渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight)
const container = ref(null)
const render = () => {

  renderer.render(scene, camera)
  requestAnimationFrame(render)
}
// // 添加立方体
// const geometry = new THREE.BoxGeometry(10, 10, 10)

// // 4_b
// var arr = ["bg1", "bg2", "bg3", "bg4", "bg5", "bg6"]
// var boxMaterials = []
// arr.forEach((item) => {
//   // 纹理加载
// let texture = new THREE.TextureLoader().load(`./living/${item}.jpg`)

//   // 创建材质
//   boxMaterials.push(new THREE.MeshBasicMaterial({ map: texture }))
// });
// const cube = new THREE.Mesh(geometry, boxMaterials)
// cube.geometry.scale(1,1,-1)
// scene.add(cube)

// 添加球
const geometry = new THREE.SphereBufferGeometry(5, 35, 35)
const loader = new THREE.TextureLoader()
loader.load('./ball/hdr/ball.hdr',(texture) => {
  const material = new THREE.MeshBasicMaterial({ map: texture })
  const sphere = new THREE.Mesh(geometry, material)
  sphere.geometry.scale(1, 1, -1)
  scene.add(sphere)
})
// 挂载完毕之后获取dom
onMounted(() => {
  // 添加控制器
  const controls = new OrbitControls(camera, container.value)
  controls.enableDamping = true
  container.value.appendChild(renderer.domElement)
  render()
})
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

.container {
  height: 100vh;
  widows: 100vw;
  background-color: #f0f0f0;
}
</style>
