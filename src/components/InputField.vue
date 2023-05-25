<template>
  <div class="form-field">
    <label :for="id" class="form-label">{{ label }}</label>
    <input
      :type="type"
      :id="id"
      class="form-input"
      :value="value"
      @input="$emit('update:setNewValue', $event.target.value)"
    />
  </div>
</template>

<script>
export default {
  props: {
    id: {
      type: String,
      required: true,
    },
    label: {
      type: String,
      required: true,
    },
    type: {
      type: String,
      default: "text",
    },
    value: {
      required: true,
    },
    required: {
      type: Boolean,
      default: false,
    },
  },
  emits: ["update:setNewValue"],
};
</script>

<style scoped>
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
</style>
