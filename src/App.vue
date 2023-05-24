<script>
import Table from "./components/Table.vue";
import Modal from "./components/Modal.vue";
import Form from "./components/Form.vue";
import axiosInstance from "./services/api-client";

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
  mounted() {
    axiosInstance
      .get("/todos")
      .then(response => {
        this.todos = response.data;
      })
      .catch(error => {
        console.log(error);
      });
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
      this.formData = this.todos[index];
    },
    addTodo() {
      this.showModal = false;
      axiosInstance
        .post("/todos", this.formData)
        .then(response => {
          this.todos.push(response.data);
        });
    },
    editTodo(id) {
      this.showModal = false;
      axiosInstance
        .put(`/todos/${id}`, this.formData)
        .then(response => {
          this.todos.forEach(todo => {
            if (todo.id === id) {
              this.todos[id - 1] = response.data;
            }
          });
        });
    },
    deleteTodo(id) {
      axiosInstance
        .delete(`/todos/${id}`)
        .then(response => {
          this.todos.forEach(todo => {
            if (todo.id === id) {
              this.todos.splice(this.todos.indexOf(todo), 1);
            }
          });
        });
    },
  },
};
</script>

<template>
  <div
    style="
      display: flex;
      justify-content: flex-end;
      margin-top: 3rem;
      margin-right: 8rem;
    "
  >
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

<style scoped></style>
