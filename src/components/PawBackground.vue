<template>
  <div class="paw-background">
    <div
      v-for="(paw, index) in pawPositions"
      :key="index"
      class="paw"
      :style="{
        top: paw.top,
        left: paw.left,
        transform: `translateY(${paw.offset}px) rotate(${paw.rotate}deg) scale(${paw.scale})`,
      }"
    ></div>
  </div>
</template>

<script>
export default {
  name: "PawBackground",
  data() {
    return {
      pawPositions: [
        { top: "-10%", left: "80%", offset: 0, rotate: 110, scale: 0.6 },
        { top: "-10%", left: "20%", offset: 0, rotate: 250, scale: 0.4 },
        { top: "-10%", left: "5%", offset: 0, rotate: 200, scale: 0.8 },
        { top: "-35%", left: "40%", offset: 0, rotate: 10, scale: 1.2 },
        { top: "-95%", left: "10%", offset: 0, rotate: 190, scale: 1.8 },
        { top: "-145%", left: "20%", offset: 0, rotate: 200, scale: 1 },
        { top: "10%", left: "80%", offset: 0, rotate: 10, scale: 1 },
        { top: "15%", left: "60%", offset: 0, rotate: 40, scale: 0.6 },
        { top: "50%", left: "40%", offset: 0, rotate: 60, scale: 1.6 },
        { top: "70%", left: "80%", offset: 0, rotate: 30, scale: 0.9 },
        { top: "90%", left: "10%", offset: 0, rotate: 240, scale: 1.4 },
        { top: "10%", left: "10%", offset: 0, rotate: 190, scale: 2 },
      ],
    };
  },
  mounted() {
    // Динамическое обновление позиции лапок при скролле
    window.addEventListener("scroll", this.updatePawPositions);
  },
  beforeUnmount() {
    window.removeEventListener("scroll", this.updatePawPositions);
  },
  methods: {
    updatePawPositions() {
      const { scrollY } = window;
      this.pawPositions = this.pawPositions.map((paw, index) => ({
        ...paw,
        offset: scrollY * (0.5 + index * 0.2), // Разная скорость движения
      }));
    },
  },
};
</script>

<style scoped>
.paw-background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none; /* Фон не мешает основному контенту */
  overflow: hidden;
  z-index: -1; /* Фон под основным контентом */
}

.paw {
  position: absolute;
  width: 100px;
  height: 100px;
  background: url("../assets/puppies_photos/paw.png") no-repeat center;
  background-size: contain;
  opacity: 1; /* Полупрозрачные лапки */
  filter: blur(2px); /* Лёгкий блюр */
  transition: transform 0.2s linear; /* Плавное движение */
}
</style>
