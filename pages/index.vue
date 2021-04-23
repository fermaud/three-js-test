<template>
  <div class="p-0">
    <div id="container" style="width: 100vw; height: calc(100vh - 10em)"></div>
    <div style="width: 100% height: 10em" class="p-4">
      {{ speedY }}
      <b-button
        @click="
          speedY = speedY - 0.01;
          animate();
        "
        >Speed -</b-button
      >
      <b-button
        @click="
          speedY = speedY + 0.01;
          animate();
        "
        >Speed +</b-button
      >
    </div>
  </div>
</template>

<script>
import * as Three from "three";

export default {
  name: "ThreeTest",
  data() {
    return {
      camera: null,
      scene: null,
      renderer: null,
      mesh: null,
      speedX: 0.01,
      speedY: 0.01
    };
  },
  methods: {
    init() {
      let container = document.getElementById("container");

      this.camera = new Three.PerspectiveCamera(
        70,
        container.clientWidth / container.clientHeight,
        0.01,
        10
      );
      this.camera.position.z = 1;

      this.scene = new Three.Scene();

      let geometry = new Three.BoxGeometry(0.2, 0.2, 0.2);
      let material = new Three.MeshNormalMaterial();

      this.mesh = new Three.Mesh(geometry, material);
      this.scene.add(this.mesh);

      this.renderer = new Three.WebGLRenderer({ antialias: true });
      this.renderer.setSize(container.clientWidth, container.clientHeight);
      container.appendChild(this.renderer.domElement);
    },
    animate() {
      requestAnimationFrame(this.animate);
      this.mesh.rotation.x += this.speedX;
      this.mesh.rotation.y += this.speedY;
      this.renderer.render(this.scene, this.camera);
    }
  },
  mounted() {
    this.init();
    this.animate();
  }
};
</script>
