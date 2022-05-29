<template>
  <div class="item--list">
      <span>Название продукта: Арбуз,</span>
      <span>Вес: {{ item.m}} кг</span>
      
  </div>
</template>

<script>
export default {
    name: 'ItemList',
    props: {
        item: {id: Number, m: Number}
    }
}
</script>

<style>
.item--list {
    padding: 10px;
    display: flex;
    border: 1px solid teal;
}
div span {
    margin-right: 10px;
}
</style>