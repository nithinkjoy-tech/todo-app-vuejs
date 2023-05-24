<script>
export default {
  props: {
    todos: Array,
    showModal: Boolean,
  },

  methods: {
    handleEdit(index) {
      this.$emit("openModal", index);
      this.$emit("setMode");
    },
    handleDelete(index) {
      this.$emit("handleDelete", index);
    },
  },
};
</script>

<template>
  <div>
    <table style="margin-top: 0; padding-top: 0">
      <thead style="margin-top: 0">
        <tr>
          <th>User ID</th>
          <th>Sl No</th>
          <th>Title</th>
          <th>Completed</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(todo, index) in todos">
          <td v-for="item in todo">
            {{ item }}
          </td>
          <td style="display: flex">
            <a @click="handleEdit(index)">Edit</a>
            <a @click="handleDelete(todo.id)">Delete</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
table,
thead,
tbody,
tr,
th,
td {
  border-collapse: separate;
  font-size: 1.5rem;
  padding: 1rem 2rem;
  border-spacing: 0 1rem;
}

a {
  text-decoration: none;
  color: #4d47c3;
  cursor: pointer;
  padding-left: 1rem;
}

tr {
  border-radius: 5px;
}

td {
  background-color: #f0efff;
}

.input-group {
  display: inline-flex;
}
.input {
  padding: 1.3rem 1rem;
  font-size: 1.3rem;
}

.completed {
  width: 1rem;
}

.title {
  width: 35rem;
}
</style>
