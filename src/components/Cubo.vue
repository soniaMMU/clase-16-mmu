<template>
  <div class="contenedor-principal">
    <div ref="cuboContainer" class="contenedor-render"></div>
  </div>
</template>

<script lang="ts" setup>
import { onMounted, ref } from 'vue'
import * as THREE from 'three'

const cuboContainer = ref<HTMLDivElement | null>(null)

onMounted(() => {
  if (!cuboContainer.value) return

  const scene = new THREE.Scene()
  const camera = new THREE.PerspectiveCamera(
    75,
    cuboContainer.value.clientWidth / cuboContainer.value.clientHeight,
    0.1,
    1000
  )
  camera.position.z = 5

  const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true })
  renderer.setSize(
    cuboContainer.value.clientWidth,
    cuboContainer.value.clientHeight
  )
  cuboContainer.value.appendChild(renderer.domElement)

  const textureLoader = new THREE.TextureLoader()
  const texture = textureLoader.load('/textura.jpg')

  const geometry = new THREE.BoxGeometry()
  const material = new THREE.MeshBasicMaterial({ map: texture })

  const cube = new THREE.Mesh(geometry, material)
  scene.add(cube)

  const raycaster = new THREE.Raycaster()
  const mouse = new THREE.Vector2()

  // Plano para seguir el mouse
  const planeZ = new THREE.Plane(new THREE.Vector3(0, 0, 1), -5) // Plano paralelo a la cámara

  function onMouseMove(event: MouseEvent) {
    const rect = renderer.domElement.getBoundingClientRect()
    mouse.x = ((event.clientX - rect.left) / rect.width) * 2 - 1
    mouse.y = -((event.clientY - rect.top) / rect.height) * 2 + 1

    raycaster.setFromCamera(mouse, camera)
    const intersection = new THREE.Vector3()
    raycaster.ray.intersectPlane(planeZ, intersection)
    cube.position.x = intersection.x
    cube.position.y = intersection.y
  }

  renderer.domElement.addEventListener('mousemove', onMouseMove)

  const animate = () => {
    requestAnimationFrame(animate)
    cube.rotation.x += 0.01
    cube.rotation.y += 0.01
    renderer.render(scene, camera)
  }

  animate()

  window.addEventListener('resize', () => {
    if (!cuboContainer.value) return
    camera.aspect =
      cuboContainer.value.clientWidth / cuboContainer.value.clientHeight
    camera.updateProjectionMatrix()
    renderer.setSize(
      cuboContainer.value.clientWidth,
      cuboContainer.value.clientHeight
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
