<template>
  <input
    type="datetime-local"
    :value="value"
    :min="min"
    :max="max"
    format-value="yyyy-MM-ddTHH:mm"
    @input="updateInput"
  />
</template>

<script>
export default {
  name: "DatePicker",
  props: {
    value: String,
    min: String,
    max: String,
  },
  methods: {
    updateInput(event) {
      this.$emit("update:value", event.target.value);
    },
  },
};
</script>

<style>
input {
  padding: 10px;
  margin-top: 10px;
  border: 1px solid teal;
}
</style>
