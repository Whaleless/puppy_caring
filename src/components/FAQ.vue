<template>
  <section class="faq">
    <h2 class="subtitle">Часто задаваемые вопросы</h2>
    <div v-for="qa in faqList" :key="qa.id" class="faq-item">
      <h3 class="question-title" @click="toggle(qa.id)">{{ qa.question }}</h3>
      <p class="answer-text" v-if="qa.open">{{ qa.answer }}</p>
    </div>
  </section>
</template>

<script>
export default {
  name: "FAQ",
  data() {
    return {
      faqList: [
        {
          id: 1,
          question: "Как правильно кормить щенка?",
          answer: "Описание питания...",
          open: false,
        },
        {
          id: 2,
          question: "Когда можно начинать гулять?",
          answer: "После завершения всех прививок...",
          open: false,
        },
        // Добавь больше вопросов
      ],
    };
  },
  methods: {
    toggle(id) {
      const faq = this.faqList.find((item) => item.id === id);
      faq.open = !faq.open;
    },
  },
};
</script>

<style scoped>
.faq {
  max-width: 90vw;
}

.faq-item {
  cursor: pointer;
}

.question-title,
.answer-text {
  padding: 15px;
}
</style>
