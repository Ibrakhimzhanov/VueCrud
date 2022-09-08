<template>
  <div id="employee-table">
    <p v-if="employees.length < 1" class="empty-table">Нет работников</p>
    <table v-else>
      <thead>
        <tr>
          <th>Имя сотрудника</th>
          <th>Почта сотрудника</th>
          <th>Действия</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="employee in employees" :key="employee.id">
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.name" />
          </td>
          <td v-else>{{ employee.name }}</td>
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.email" />
          </td>
          <td v-else>{{ employee.email }}</td>
          <td v-if="editing === employee.id">
            <button @click="editEmployee(employee)">Сохранить</button>
            <button class="muted-button" @click="cancelEdit(employee)">
              Отмена
            </button>
          </td>
          <td v-else>
            <button @click="editMode(employee.id)">Изменить</button>
            <button @click="$emit('delete:employee', employee.id)">
              Удалить
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "employee-table",
  props: {
    employees: Array,
  },
  data() {
    return {
      editing: null,
    };
  },
  methods: {
    editMode(emp) {
      this.cachedEmployee = Object.assign({}, emp);
      this.editing = emp.id;
    },
    cancelEdit(emp) {
      Object.assign(emp, this.cachedEmployee);
      this.editing = null;
    },
    editEmployee(emp) {
      if (emp.name === "" || emp.email === "") return;
      this.$emit("edit:emp", emp.id, emp);
      this.editing = null;
    },
  },
};
</script>

<style scoped>
button {
  margin-right: 0.5rem;
}
</style>
