<template>
  <form @submit.prevent>
    <p class="error" v-if="errors.has('melons')">{{ errors.get("melons") }}</p>
    <CustomButton style="margin-top: 10px" @click="selectButton">{{
      form.pickedMelons.length > 0 ? "Изменить выбор" : "Выбрать арбузы"
    }}</CustomButton>
    <div v-if="isSelecting" class="picker">
      <PlacePicker @melonsPicked="melonsPicked" />
    </div>
    <div v-if="showItems" class="items">
      <p>Список выбранных арбузов</p>
      <ItemList
        class="item"
        v-for="item in form.pickedMelons"
        :key="item.id"
        :item="item"
      />
    </div>
    <p class="error" v-if="errors.has('adress')">{{ errors.get("adress") }}</p>
    <InputField
      v-model:value="form.adress"
      type="text"
      placeholder="Адрес доставки"
    />
    <p class="error" v-if="errors.has('phone')">{{ errors.get("phone") }}</p>
    <InputField
      v-model:value="form.phone"
      type="tel"
      placeholder="Номер телефона"
    />
    <p class="error" v-if="errors.has('date')">{{ errors.get("date") }}</p>
    <DatePicker v-model:value="form.dateValue" :min="minDate" :max="maxDate" />
    <div class="checkbox-line">
      <input type="checkbox" id="checkbox" v-model="form.cut" />
      <label for="checkbox">Порезать дольками</label>
    </div>
    <CustomButton style="margin-top: 10px" @click="sendOrder"
      >Заказать</CustomButton
    >
  </form>
</template>

<script>
import InputField from "./components/InputField.vue";
import CustomButton from "./components/CustomButton.vue";
import PlacePicker from "./components/placePicker/PlacePicker.vue";
import DatePicker from "./components/DatePicker.vue";
import ItemList from "./components/ItemList.vue";
export default {
  name: "App",
  components: {
    InputField,
    CustomButton,
    PlacePicker,
    DatePicker,
    ItemList,
  },
  data() {
    return {
      form: {
        cut: false,
        phone: "",
        adress: "",
        pickedMelons: [],
        dateValue: "",
      },
      isSelecting: false,
      showItems: false,
      minDate: "",
      maxDate: "",
      errors: new Map(),
    };
  },
  methods: {
    selectButton() {
      this.isSelecting = true;
    },
    melonsPicked(melons) {
      this.form.pickedMelons = [];
      melons.forEach((v) => this.form.pickedMelons.push(v));
      this.showItems = true;
      this.isSelecting = false;
    },
    getMinDate(date = new Date()) {
      date.setTime(date.getTime() + 8 * 60 * 60 * 1000);
      this.minDate = new Date(date).toJSON().slice(0, 16);
    },
    getMaxDate(date = new Date()) {
      date.setDate(date.getDate() + 9);
      this.maxDate = new Date(date).toJSON().slice(0, 16);
    },
    sendOrder() {
      if (this.checkForm()) {
        console.log(this.form);
      }
    },
    checkForm() {
      if (
        this.form.phone &&
        this.form.adress &&
        this.form.pickedMelons.length != 0 &&
        this.form.dateValue
      ) {
        return true;
      }
      if (!this.form.phone || isNaN(this.form.phone)) {
        this.errors.set("phone", "Введите корректный номер телефона");
      } else {
        this.errors.delete("phone");
      }
      if (!this.form.adress) {
        this.errors.set("adress", "Введите адрес");
      } else {
        this.errors.delete("adress");
      }
      if (this.form.pickedMelons.length == 0) {
        this.errors.set("melons", "Выберите арбузы");
      } else {
        this.errors.delete("melons");
      }
      if (!this.form.dateValue) {
        this.errors.set("date", "Выберите время и дату доставки");
      } else {
        this.errors.delete("date");
      }
    },
  },
  mounted() {
    this.getMinDate(), this.getMaxDate();
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

form {
  margin: 10px;
}
.items {
  margin-top: 10px;
}
.item {
  margin-top: 10px;
}

.picker {
  margin-top: 10px;
}

.error {
  margin-top: 10px;
  color: red;
}
</style>
