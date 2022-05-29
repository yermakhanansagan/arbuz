<template>
  <p>Выберите место арбуза, можно выбрать максимум 3</p>
  <div class="grid" :style="cssVars">
    <Cell
      v-for="item in waterMelons"
      :item="item"
      :key="item.id"
      @click="onClick(item)"
      :isSelected="selectedItems.has(item)"
    />
  </div>
  <div class="colors" :style="cssVars">
    <div class="colors--item">
      <div class="melon ripe"></div>
      <p>- Спелый</p>
    </div>
    <div class="colors--item">
      <div class="melon unripe"></div>
      <p>- Не спелый</p>
    </div>
    <div class="colors--item">
      <div class="melon collected"></div>
      <p>- Собран</p>
    </div>
    <div class="colors--item">
      <div class="melon clicked"></div>
      <p>- Выбран</p>
    </div>
  </div>

  <CustomButton style="margin-top: 10px" @click="nextButton"
    >Дальше</CustomButton
  >
</template>

<script>
import Cell from "./Cell.vue";
import CustomButton from "/src/components/CustomButton.vue";
export default {
  name: "PlacePicker",
  components: {
    Cell,
    CustomButton,
  },
  data() {
    return {
      waterMelons: [
        { id: 1, m: 3.5, status: "ripe" },
        { id: 2, m: 3.6, status: "unripe" },
        { id: 3, m: 3.7, status: "collected" },
        { id: 4, m: 3.3, status: "ripe" },
        { id: 5, m: 3.0, status: "ripe" },
        { id: 6, m: 3.9, status: "ripe" },
        { id: 7, m: 3.4, status: "ripe" },
        { id: 8, m: 3.8, status: "unripe" },
        { id: 9, m: 3.7, status: "collected" },
        { id: 10, m: 3.3, status: "ripe" },
      ],
      rows: 2,
      cols: 5,
      selectedItems: new Set(),
      selectedMax: 3,
    };
  },
  computed: {
    cssVars() {
      return { "--rows": this.rows, "--cols": this.cols };
    },
  },
  methods: {
    onClick(item) {
      if (!this.selectedItems.has(item)) {
        if (this.selectedItems.size < this.selectedMax) {
          this.selectedItems.add(item);
        }
      } else {
        this.selectedItems.delete(item);
      }
    },
    nextButton() {
      if (this.selectedItems.size != 0) {
        this.$emit("melonsPicked", this.selectedItems);
      }
    },
  },
};
</script>

<style>
.grid {
  display: grid;
  grid-template-columns: repeat(var(--cols), 50px);
  grid-template-rows: repeat(var(--rows), 50px);
  gap: 3px;
  margin-top: 10px;
}
.colors {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
  flex-wrap: wrap;
  gap: 5PX
}
.colors--item {
  display: flex;
  align-items: center;
  margin-right: 10px;
}
.melon {
  border: 1px solid teal;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  cursor: pointer;
  margin-right: 3px;
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
