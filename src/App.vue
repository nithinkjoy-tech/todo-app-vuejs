<script>
import Table from "./components/Table.vue";
import Modal from "./components/Modal.vue";
import Form from "./components/Form.vue";
import APIClient from "./services/api-client";

const apiClient = new APIClient("/todos");

export default {
  components: {
    Table,
    Modal,
    Form,
  },
  data() {
    return {
      showModal: false,
      todos: [],
      formData: {
        userId: "",
        id: "",
        title: "",
        completed: "",
      },
      mode: "",
    };
  },
  async beforeMount() {
    let data = await apiClient.get();
    this.todos = data;
  },

  methods: {
    handleSubmit(id) {
      if (this.mode == "edit") this.editTodo(id);
      if (this.mode == "add") this.addTodo();
    },

    addDataHelper() {
      this.showModal = true;
      this.formData = {
        userId: "",
        id: "",
        title: "",
        completed: "",
      };
      this.mode = "add";
    },

    editDataHelper(index) {
      this.showModal = true;
      this.formData = JSON.parse(JSON.stringify(this.todos[index]));
    },

    async addTodo() {
      this.showModal = false;
      let data = await apiClient.post(this.formData);
      this.todos.push(data);
    },

    async editTodo(id) {
      this.showModal = false;
      let data = await apiClient.put(id, this.formData);
      this.todos.forEach(todo => {
        if (todo.id === data.id) {
          this.todos[id - 1] = data;
        }
      });
    },

    async deleteTodo(id) {
      await apiClient.delete(id);
      this.todos.forEach(todo => {
        if (todo.id === id) {
          this.todos.splice(this.todos.indexOf(todo), 1);
        }
      });
    },
  },
};
</script>

<template>
  <div class="button-container">
    <button class="btn btn-primary" @click="addDataHelper">Add</button>
  </div>
  <Teleport to="body">
    <modal :show="showModal" @close="showModal = false">
      <Form
        @handleSubmit="id => handleSubmit(id)"
        :formData="this.formData"
        @close="showModal = false"
      />
    </modal>
  </Teleport>
  <Table
    @setMode="mode = 'edit'"
    @openModal="editDataHelper"
    @handleDelete="id => deleteTodo(id)"
    :todos="this.todos"
  />
</template>

<style scoped>
.button-container {
  display: flex;
  justify-content: flex-end;
  margin-top: 3rem;
  margin-right: 8rem;
}
</style>
