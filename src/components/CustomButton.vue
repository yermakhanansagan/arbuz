<template>
  <button class="btn">
      <slot></slot>
  </button>
</template>

<script>
export default {
    name: 'CustomButton'
}
</script>

<style>
    .btn {
        background: none;
        padding: 10px 15px;
        border: 1px solid teal;
        cursor: pointer;
    }
</style>