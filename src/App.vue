<script setup>
import {ref} from "vue";
import {onBeforeRender} from "lunchboxjs";

const now = ref(Date.now() * 0.001)
const earthRotation = ref({ y: 0 })
const moonRotation = ref({ y: 0})

onBeforeRender(() => {
  now.value = Date.now() * 0.001
  earthRotation.value.y = now.value * 2.8
  moonRotation.value.y = now.value * 0.1
})
</script>

<template>
  <Lunchbox :camera-position="[0, 0, 20]" :camera-look="[0, 0, 0]" shadow>
    <directionalLight :intensity="1" :position="[0, 0, 20]"/>
    <ambientLight :intensity="0.3"/>
    <mesh :position-z="-10">
      <planeGeometry :args="[50, 25]"/>
      <meshPhysicalMaterial>
        <textureLoader src="/2k_stars_milky_way.jpeg" attach="map"/>
      </meshPhysicalMaterial>
    </mesh>
    <mesh :rotation-y="earthRotation.y">
      <sphereGeometry :args="[1, 32, 32]"/>
      <meshPhysicalMaterial>
        <textureLoader src="/map-of-earth.avif" attach="map"/>
        <textureLoader src="/grayscale-map-of-earth.avif" attach="bumpMap"/>
      </meshPhysicalMaterial>
    </mesh>
    <mesh :position-z="Math.cos(now) * 10" :position-x="Math.sin(now) * 10" :scale="0.25" :rotation-y="moonRotation.y">
      <sphereGeometry :args="[1, 32, 32]"/>
      <meshPhysicalMaterial>
        <textureLoader src="/moon-map.jpeg" attach="map"/>
        <textureLoader src="/moon-bump-map.jpeg" attach="bumpMap"/>
      </meshPhysicalMaterial>
    </mesh>
  </Lunchbox>
</template>
