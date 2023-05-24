<script>
export default {
  props: {
    formData: Object,
  },
};
</script>

<template>
  <div class="form-container">
    <form @submit.prevent="submitForm">
      <div class="form-field">
        <label for="userId" class="form-label">User ID:</label>
        <input type="text" id="userId" class="form-input" v-model="formData.userId" />
      </div>
      <div class="form-field">
        <label for="slNo" class="form-label">Sl No:</label>
        <input type="text" id="slNo" class="form-input" v-model="formData.id" />
      </div>
      <div class="form-field">
        <label for="title" class="form-label">Title:</label>
        <input type="text" id="title" class="form-input" v-model="formData.title" />
      </div>
      <div class="form-field">
        <label for="completed" class="form-label">Completed:</label>
        <select id="completed" class="form-input" v-model="formData.completed">
          <option value="true">Yes</option>
          <option value="false">No</option>
        </select>
      </div>
      <div class="form-field button-group">
        <button
          type="submit"
          @click="$emit('handleSubmit',formData?.id)"
          class="form-button submit"
        >
          Submit
        </button>
        <button @click="$emit('close')" class="form-button cancel">
          Cancel
        </button>
      </div>
    </form>
  </div>
</template>

<style scoped>
.form-container {
  width: 400px;
  margin: 0 auto;
}

.form-field {
  margin-bottom: 20px;
}

.form-label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
}

.form-input {
  width: 100%;
  padding: 10px;
  font-size: 14px;
}

.form-button {
  padding: 10px 20px;
  color: white;
  border: none;
  cursor: pointer;
}

.submit {
  background-color: #4caf50;
  transition: background-color 0.3s ease;
}

.submit:hover {
  filter: brightness(1.2);
}

.cancel {
  background-color: #03a9f4;
  transition: background-color 0.3s ease;
}
.cancel:hover {
  filter: brightness(1.2);
}

.button-group {
  display: flex;
  justify-content: space-between;
}
</style>
