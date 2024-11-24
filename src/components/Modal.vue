<template>
  <transition name="fade">
    <div class="modal-overlay" v-if="showModal">
      <div class="modal-content">
        <h2 class="modal-title">Введите имя щенка</h2>
        <hr />
        <form class="form" @submit.prevent="submitPuppyName">
          <input
            v-model="puppyName"
            :class="{ error: hasError }"
            placeholder="Имя щенка"
            @input="clearError"
          />
          <button type="submit">Подтвердить</button>
        </form>
        <p v-if="hasError" class="error-message">
          Имя щенка не найдено, попробуйте снова.
        </p>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: "ModalWindow",
  props: {
    puppiesData: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      puppyName: "",
      hasError: false,
      showModal: true,
    };
  },
  methods: {
    submitPuppyName() {
      const puppyIndex = this.puppiesData.findIndex(
        (puppy) =>
          puppy.name.toLowerCase() === this.puppyName.trim().toLowerCase(),
      );

      if (puppyIndex !== -1) {
        this.$emit("code-submitted", puppyIndex);
        localStorage.setItem("puppyIndex", puppyIndex);
        this.showModal = false;
      } else {
        this.hasError = true;
      }
    },
    clearError() {
      this.hasError = false;
    },
  },
};
</script>

<style scoped>
/* Анимация появления и исчезновения */
/* Стили модального окна */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.4);
  backdrop-filter: blur(5px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1;
}

.modal-title {
  margin-top: 5px;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 8px;
  text-align: center;
}

.form {
  display: flex;
  margin-top: 10px;
  height: 40px;
}

input {
  margin-right: 5px;
  padding: 8px;
  width: 80%;
  border: 1px solid #ccc;
  border-radius: 4px;
}

input.error {
  border-color: #e63946;
}

.error-message {
  color: #e63946;
  margin-top: 10px;
}

button {
  padding: 10px 20px;
  background-color: #4a4e69;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

@media screen and (max-width: 600px) {
  .modal-content {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 30%;
  }
}
</style>
