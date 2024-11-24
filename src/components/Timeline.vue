<template>
  <section>
    <h2 class="subtitle">Таймлайн ухода</h2>
    <div class="timeline-container">
      <!-- Кнопка "Назад" -->
      <button
        class="arrow-button to-left"
        @click="prevEvents"
        :disabled="currentIndex === 0"
      />
      <!-- События таймлайна -->
      <div
        v-for="event in visibleTimeline"
        :key="event.id"
        class="timeline-event"
      >
        <h3>{{ event.months }} мес.</h3>
        <p>{{ event.description }}</p>
        <div class="to-bottom">
          <div class="progress-bar">
            <div
              class="progress-bar-fill"
              :style="{ width: getProgress(event.days) + '%' }"
            ></div>
          </div>
          <p class="remaining-time">
            Осталось: {{ getRemainingTime(event.days) }}
          </p>
        </div>
      </div>

      <!-- Кнопка "Вперед" -->
      <button
        class="arrow-button to-right"
        @click="nextEvents"
        :disabled="currentIndex + eventsPerPage >= timeline.length"
      />
    </div>
  </section>
</template>
<script>
export default {
  name: "TimelineBlock",
  props: {
    currentAgeInDays: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      timeline: [
        {
          id: 1,
          days: 90,
          months: 3,
          description: "Вакцинация. Бешенство. Ежегодно",
        },
        {
          id: 2,
          days: 90,
          months: 3,
          description: "Вакцинация. Общие, 1-ая фаза",
        },
        {
          id: 3,
          days: 105,
          months: 3.5,
          description: "Вакцинация. Общие, 2-ая фаза",
        },
        { id: 4, days: 120, months: 4, description: "Первые прогулки." },
        { id: 5, days: 120, months: 4, description: "Чипирование." },
        {
          id: 6,
          days: 150,
          months: 5,
          description: "Молочные зубы активно выпадают.",
        },
        {
          id: 8,
          days: 435,
          months: 14.5,
          description: "Вакцинация. Бешенство.",
        },
      ],
      currentIndex: 0,
      eventsPerPage: 3,
    };
  },
  computed: {
    computedIndex() {
      if (!this.currentAgeInDays) {
        return 0; // По умолчанию отображаем первый элемент
      }
      const index = this.timeline.findIndex(
        (event) => event.days > this.currentAgeInDays,
      );
      return index === -1 ? this.timeline.length - 1 : index;
    },
    visibleTimeline() {
      return this.timeline.slice(
        this.currentIndex,
        this.currentIndex + this.eventsPerPage,
      );
    },
  },
  watch: {
    currentAgeInDays(newAge) {
      if (newAge) {
        this.currentIndex = this.computedIndex; // Обновляем `currentIndex`, когда возраст доступен
      }
    },
  },
  mounted() {
    this.currentIndex = this.computedIndex; // Устанавливаем стартовый индекс
  },
  methods: {
    nextEvents() {
      if (this.currentIndex + this.eventsPerPage < this.timeline.length) {
        this.currentIndex += this.eventsPerPage;
      }
    },
    prevEvents() {
      if (this.currentIndex > 0) {
        this.currentIndex -= this.eventsPerPage;
      }
      if (this.currentIndex < 0) this.currentIndex = 0;
    },
    getProgress(eventAgeInDays) {
      const percentage = (this.currentAgeInDays / eventAgeInDays) * 100;
      return Math.min(percentage, 100).toFixed(0);
    },
    getRemainingTime(eventAgeInDays) {
      const remainingDays = eventAgeInDays - this.currentAgeInDays;
      if (remainingDays <= 0) return "Уже наступило";
      return `${remainingDays} дней`;
    },
  },
};
</script>
<style scoped>
.timeline-container {
  display: flex;
  align-items: center;
}

.timeline-event {
  display: flex;
  flex-direction: column;
  justify-content: space-around;

  height: 200px;
  width: 100%;
  margin: 10px 6px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 20px;
  text-align: center;
}

.arrow-button {
  background-image: url("../assets/images/arrow.png");
  background-color: transparent;
  background-size: contain;
  background-repeat: no-repeat;
  border: none;
  font-size: 24px;
  cursor: pointer;
  padding: 25px;
  margin: 0 15px;
}

.arrow-button:disabled {
  cursor: not-allowed;
}

.arrow-button.to-right {
  transform: rotate(-180deg);
}

.progress-bar {
  width: 100%;
  height: 10px;
  background: #eee;
  border-radius: 5px;
  margin: 10px 0;
  overflow: hidden;
}

.progress-bar-fill {
  height: 100%;
  background: lightgreen;
  transition: width 0.3s ease;
}

.remaining-time {
  font-size: 14px;
  color: #666;
}

@media (max-width: 600px) {
  .timeline-container {
    flex-direction: column;
  }

  .arrow-button.to-right {
    transform: rotate(-90deg);
  }

  .arrow-button.to-left {
    transform: rotate(90deg);
  }

  .arrow-button:disabled {
    display: none;
  }
}
</style>
