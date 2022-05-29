<template>
  <form @submit.prevent>
    <p class="error" v-if="errors.has('melons')">{{ errors.get("melons") }}</p>
    <CustomButton style="margin-top: 10px" @click="selectButton">{{
      form.pickedMelons.length > 0 ? "Изменить выбор" : "Выбрать арбузы"
    }}</CustomButton>
    <div v-if="isSelecting" class="picker">
      <PlacePicker @melonsPicked="melonsPicked" :waterMelons="waterMelons"/>
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
    <div class="accepted" v-if="accepted">
      <p>Вы заказали</p>
      {{ form }}
      /></div>
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
      accepted: false
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
      this.errors.delete("melons")
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
        this.accepted = true
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
      if (this.form.phone.length == 0) {
        this.errors.set("phone", "Введите корректный номер телефона");
      } else {
        if (typeof this.form.phone === 'number'){
          this.errors.set("phone", "Введите корректный номер телефона");
        }
        this.errors.delete("phone");
      }this.errors.delete("phone");
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
.accepted {
  margin-top: 10px;
}
</style>
