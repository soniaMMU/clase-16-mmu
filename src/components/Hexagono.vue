<template>
  <div class="contenedor-principal">
    <div ref="hexagonoContainer" class="contenedor-render"></div>
  </div>
</template>

<script lang="ts" setup>
import { onMounted, ref } from 'vue'
import * as THREE from 'three'

const hexagonoContainer = ref<HTMLDivElement | null>(null)

onMounted(() => {
  if (!hexagonoContainer.value) return

  const scene = new THREE.Scene()
  const camera = new THREE.PerspectiveCamera(
    75,
    hexagonoContainer.value.clientWidth / hexagonoContainer.value.clientHeight,
    0.1,
    1000
  )
  camera.position.z = 5

  const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true })
  renderer.setSize(
    hexagonoContainer.value.clientWidth,
    hexagonoContainer.value.clientHeight
  )
  hexagonoContainer.value.appendChild(renderer.domElement)

  const textureLoader = new THREE.TextureLoader()
  const texture = textureLoader.load('/images.jpg')

  const geometry = new THREE.CylinderGeometry(1, 1, 2, 6)
  const material = new THREE.MeshBasicMaterial({ map: texture })

  const hexagon = new THREE.Mesh(geometry, material)
  scene.add(hexagon)

  const animate = () => {
    requestAnimationFrame(animate)
    hexagon.rotation.x += 0.01
    hexagon.rotation.y += 0.01
    renderer.render(scene, camera)
  }

  animate()

  window.addEventListener('resize', () => {
    if (!hexagonoContainer.value) return
    camera.aspect =
      hexagonoContainer.value.clientWidth / hexagonoContainer.value.clientHeight
    camera.updateProjectionMatrix()
    renderer.setSize(
      hexagonoContainer.value.clientWidth,
      hexagonoContainer.value.clientHeight
    )
  })
})
</script>

<style scoped>
.contenedor-principal {
  width: 100vw;
  height: 90vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.contenedor-render {
  width: 90%;
  height: 100%;
}
</style>
