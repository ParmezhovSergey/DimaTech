<template>
  <div class="tableForm">
    <div v-if="modal" class="form">
      <div class="content">
        <div class="name">
          <label for="name">Наименование:</label>
          <input type="text" id="name" v-model="inputFields.name" required />
        </div>
        <div class="value">
          <label for="value">Значение:</label>
          <input
            type="number"
            id="value"
            min="0"
            step="1"
            v-model="inputFields.value"
            required
            style="margin-left: 34px"
          />
        </div>
        <div class="color">
          <label for="color"
            >Цвет:
            <input
              type="text"
              readonly
              v-model="inputFields.color"
              style="margin-left: 60px"
            />
          </label>
          <button
            :style="{ background: inputFields.color }"
            @click="
              colorPick.showcolorpicker = colorPick.showcolorpicker
                ? false
                : true
            "
          >
            &nbsp;
          </button>
          <div v-show="colorPick.showcolorpicker" style="position: absolute">
            <ColorPicker
              theme="light"
              :sucker-hide="false"
              @changeColor="changeColor"
            />
          </div>
        </div>
        <button class="btn" @click="saveEntry">Сохранить</button>
        <button class="cancellation" @click="cancel">Отмена</button>
      </div>
    </div>

    <div>
      <div class="table" v-for="(entry, id) in props.entries" :key="id">
        <div style="width: 170px; border-right: 1px solid gray">
          {{ entry.name }}
        </div>
        <div style="width: 50px; border-right: 1px solid gray">
          {{ entry.value }}%
        </div>
        <div style="width: 130px; margin-left: 5px">
          <div
            style="width: 20px; height: 20px; border-radius: 50%"
            :style="{ backgroundColor: entry.color }"
          ></div>
        </div>

        <div>
          <button
            style="margin-right: 10px"
            @click="editEntry(entry.id, entry)"
            title="Редактировать"
          >
            <i class="fa fa-pencil"></i>
          </button>
        </div>
        <div>
          <button @click="deleteEntry(entry.id)" title="Удалить">
            <i class="fa fa-trash-o"></i>
          </button>
        </div>
      </div>

      <button class="btn" @click="submitForm">Добавить сектор</button>
    </div>
  </div>
</template>
<script setup>
import { ref } from "vue";
import { ColorPicker } from "vue-color-kit";
import "vue-color-kit/dist/vue-color-kit.css";

const emit = defineEmits(["add", "delete", "edit"]);

const props = defineProps({
  entries: Array,
});

const inputFields = ref({
  id: null,
  name: "",
  value: null,
  color: "#59c7f9",
});

const newFields = ref({
  id: null,
  name: "Сектор-",
  value: 0,
  color: "#59c7f9",
});

const colorPick = ref({
  showcolorpicker: false,
  color: "#59c7f9",
});

const modal = ref(false);

const clearForm = () => {
  inputFields.value.id = null;
  inputFields.value.name = "";
  inputFields.value.value = null;
  inputFields.value.color = "#59c7f9";
  colorPick.value.color = "#59c7f9";
};

const cancel = () => {
  modal.value = false;
  clearForm();
};

const submitForm = () => {
  newFields.value.id = Date.now();
  emit("add", newFields.value);
  newFields.value.id = null;
};

const changeColor = (color) => {
  const { r, g, b, a } = color.rgba;
  inputFields.value.color = `rgba(${r}, ${g}, ${b}, ${a})`;
  colorPick.value.color = `rgba(${r}, ${g}, ${b}, ${a})`;
};

const saveEntry = () => {
  emit("edit", inputFields.value);
  modal.value = false;
};

const editEntry = (id, entry) => {
  inputFields.value = { ...entry };
  modal.value = true;
};

const deleteEntry = (id) => {
  emit("delete", id);
};
</script>
<style scoped>
.table {
  display: flex;
  background-color: rgb(219, 218, 218);
  margin-bottom: 5px;
  border-radius: 5px;
  padding: 7px;
  text-align: center;
}

.btn {
  color: white;
  background-color: rgb(52, 117, 239);
  border: none;
  border-radius: 5px;
  height: 30px;
  width: 100%;
  margin-top: 10px;
}
.btn:hover{
 
  background-color: rgb(14, 92, 238);
 
}
.cancellation {
  color: white;
  background-color: rgb(248, 140, 25);
  border: none;
  border-radius: 5px;
  height: 30px;
  width: 100%;
  margin-top: 10px;
}
.form {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5); /* Затемнение */
  display: flex;
  justify-content: center;
  align-items: center;
}
.content {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}
.name {
}
.value {
  margin-top: 10px;
}
.color {
  margin-top: 10px;
}
</style>
