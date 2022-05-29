<template>
  <input class="input" type="text" :value="value" @input="updateInput">
</template>

<script>
export default {
  name: 'InputField',
  props: {
    value: [String, Number],
  },
  methods: {
    updateInput(event) {
      this.$emit('update:value', event.target.value)
    }
  }
}
</script>

<style>
.input {
  width: 100%;
  border: 1px solid teal;
  padding: 10px 15px;
  margin-top: 10px;
}
</style>