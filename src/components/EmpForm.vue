<template>
  <div id="emp-form">
    <form @submit.prevent="handleSubmit">
      <label>Имя сотрудника</label>
      <input
        ref="first"
        v-model="emp.name"
        type="text"
        :class="{ 'has-error': submitting && invalidName }"
        @focus="clearStatus"
        @keypress="clearStatus"
      />

      <label>Почта сотрудника</label>
      <input
        v-model="emp.email"
        type="text"
        :class="{ 'has-error': submitting && invalidEmail }"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">
        ❗Пожалуйста, заполните все обязательные поля
      </p>
      <p v-if="success" class="success-message">
        ✅ Сотрудник успешно добавлен
      </p>
      <button>Добавить сотрудника</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "emp-form",
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      emp: {
        name: "",
        email: "",
      },
    };
  },
  methods: {
    handleSubmit() {
      this.submitting = true;
      this.clearStatus();

      if (this.invalidName || this.invalidEmail) {
        this.error = true;
        return;
      }
      this.$emit("add:emp", this.emp);
      this.$refs.first.focus();
      this.emp = {
        name: "",
        email: "",
      };
      this.success = true;
      this.error = false;
      this.submitting = false;
    },
    clearStatus() {
      this.success = false;
      this.error = false;
    },
  },
  computed: {
    invalidName() {
      return this.emp.name === "";
    },
    invalidEmail() {
      return this.emp.email === "";
    },
  },
};
</script>

<style>
form {
  margin-bottom: 2rem;
}
[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>
