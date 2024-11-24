<template>
  <transition name="fade">
    <Modal
      v-if="isInitialized && !codeSubmitted"
      @code-submitted="handleCodeSubmission"
      :puppiesData="puppiesData"
    />
  </transition>
  <main :class="{ blurred: !codeSubmitted && isInitialized }">
    <PawBackground />

    <div v-if="isInitialized" class="main-info-container">
      <section
        class="about-puppy"
        :style="{ backgroundImage: `url(${selectedPuppy.photo})` }"
        v-if="selectedPuppy"
      >
        <div class="full-text-puppy-info">
          <h2 class="name-title">{{ selectedPuppy.name }}</h2>
          <div class="bone-info">
            <p>
              Пол: {{ selectedPuppy.gender === "male" ? "Мальчик" : "Девочка" }}
            </p>
            <p>Возраст: {{ stringedAge }}</p>
            <p>Дата рождения: {{ selectedPuppy.birthDate }}</p>
          </div>
        </div>
      </section>
      <TimelineBlock :currentAgeInDays="selectedPuppyAgeInDays" />
      <FAQ />
    </div>
  </main>
</template>

<script>
import Modal from "./components/Modal.vue";
import TimelineBlock from "./components/Timeline.vue";
import PawBackground from "./components/PawBackground.vue";
import FAQ from "./components/FAQ.vue";
// eslint-disable-next-line import/extensions
import puppiesData from "./assets/puppiesData.js";

export default {
  components: {
    Modal,
    TimelineBlock,
    FAQ,
    PawBackground,
  },
  data() {
    return {
      isInitialized: false,
      codeSubmitted: false,
      selectedPuppyIndex: null,
      puppiesData, // Импортируем массив щенков
    };
  },
  computed: {
    selectedPuppy() {
      return this.selectedPuppyIndex !== null
        ? this.puppiesData[this.selectedPuppyIndex]
        : null;
    },
    stringedAge() {
      if (!this.selectedPuppy) return null;
      const age = this.getAge(this.selectedPuppy.birthDate);
      return `${age.years > 0 ? `${age.years} лет, ` : ""}${age.months} мес, ${age.days} дн.`;
    },
    selectedPuppyAgeInDays() {
      if (!this.selectedPuppy) return 0;
      const age = this.getAge(this.selectedPuppy.birthDate);
      return age.years * 365 + age.months * 30 + age.days;
    },
  },
  mounted() {
    const savedPuppyIndex = localStorage.getItem("puppyIndex");
    if (savedPuppyIndex !== null && this.puppiesData[savedPuppyIndex]) {
      this.handleCodeSubmission(Number(savedPuppyIndex));
    }
    this.isInitialized = true;
  },
  methods: {
    handleCodeSubmission(index) {
      this.selectedPuppyIndex = index;
      this.codeSubmitted = true;
    },
    getAge(birthDate) {
      const birth = new Date(birthDate);
      const now = new Date();

      let years = now.getFullYear() - birth.getFullYear();
      let months = now.getMonth() - birth.getMonth();
      let days = now.getDate() - birth.getDate();

      if (months < 0) {
        years -= 1;
        months += 12;
      }
      if (days < 0) {
        months -= 1;
        const prevMonth = new Date(now.getFullYear(), now.getMonth(), 0);
        days += prevMonth.getDate();
      }

      return { years, months, days };
    },
  },
};
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 1.5s ease;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}

main.blurred {
  filter: blur(10px);
}

.full-text-puppy-info {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-end;
  position: absolute;
  width: 40%;
  height: 100%;
  right: 0;
}

.name-title {
  font-size: 3.5em;
}

.bone-info {
  display: flex;
  text-align: center;
  flex-direction: column;
  justify-content: center;
  position: relative;
  bottom: -70px;
  right: 64px;
  aspect-ratio: 6 / 4;
  width: 130%;
  min-width: 380px;
  background: url(assets/images/bone.png) no-repeat center / contain;
  transform: rotate(350deg);
}

.name-title {
  position: absolute;
  top: 22%;
  right: 20%;
  margin-right: 20px;
  color: #e0a149;
}

.main-info-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.about-puppy {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  position: relative;
  max-width: 890px;
  height: 65vh;
  padding: 20px;
  margin: 0 15px;
  background: transparent;
  background-size: 60%;
  background-position: left;
  background-repeat: no-repeat;
  color: white;
  z-index: 0;
}

img {
  max-width: 100%;
  border-radius: 8px;
}

@media (max-width: 600px) {
  .full-text-puppy-info {
    width: 100%;
    align-items: center;
    justify-content: space-between;
  }

  .about-puppy {
    margin: 0;
    padding: 0;
    height: 75vh;
    background-size: contain;
  }

  .name-title,
  .bone-info {
    position: relative;
    bottom: auto;
    left: auto;
    right: auto;
    margin: 0;
    padding: 0;
    width: auto;
  }

  .name-title {
    top: 5%;
  }
}
</style>
