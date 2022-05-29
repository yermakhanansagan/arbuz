<template>
  <div
    class="box"
    v-bind:class="{
      clicked: isSelected,
      ripe: item.status == 'ripe',
      unripe: item.status == 'unripe',
      collected: item.status == 'collected',
    }"
  >
    {{ item.m ? item.m + " кг" : "" }}
  </div>
</template>

<script>
export default {
  name: "Cell",
  props: {
    item: { id: Number, m: Number, status: String },
    isSelected: Boolean,
  },
};
</script>

<style>
.box {
  border: 1px solid teal;
  border-radius: 50%;
  display: grid;
  place-items: center;
  cursor: pointer;
}
.clicked {
  background: #21ba45 !important;
}
.ripe {
  background: white;
}
.unripe {
  background: #f55555;
  pointer-events: none;
}
.collected {
  background: gray;
  pointer-events: none;
}
</style>
