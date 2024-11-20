<template>
  <div class="wrapper">
    <div class="upper-block">
      <div class="upper-block__segment">
        <div class="upper-block__segment__items">
          <ItemComponent
            v-for="item in pickedItemsArr"
            :key="item?.name"
            :data-id="item?.id"
            :itemName="item?.name"
            @click="removePickedItem"
          />
        </div>
        <p>Selected: {{ pickedItemsArr?.length || "N/M" }}</p>
      </div>
      <div class="upper-block__segment">
        <div
          :class="['picked-item', pickedItem?.id ? 'selected' : '']"
          @click="(e) => removePickedItem(e, true)"
        >
          {{ pickedItem?.name }}
        </div>
      </div>
    </div>
    <div class="lower-block">
      <div class="lower-block__segment">
        <ItemComponent
          v-for="item in multiplePickItems"
          :key="item?.name"
          :data-id="item?.id"
          :itemName="item?.name"
          @click="onMultipleItemsPick"
        />
      </div>
      <div class="lower-block__segment">
        <ItemComponent
          v-for="item in singlePickItems"
          :key="item?.name"
          :data-id="item?.id"
          :itemName="item?.name"
          :class="[item?.id === parseInt(pickedItem?.id) ? 'selected' : '']"
          @click="onSingleItemPick"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import ItemComponent from "@/components/ItemComponent.vue";
import { multiplePickItems, singlePickItems } from "@/models/items.js";
import { ref } from "vue";

const pickedItemsArr = ref([]);
const pickedItem = ref(null);

function onMultipleItemsPick(e) {
  if (e.target.classList.contains("selected")) {
    removePickedItem(e);
    return;
  }
  if (pickedItemsArr.value.length < 6) {
    const id = parseInt(e.target.dataset.id);
    if (isNaN(id)) return;
    const item = multiplePickItems.find((el) => el.id === id);
    if (!item) return;
    pickedItemsArr.value.push(item);
    e.target.classList.toggle("selected");
  } else {
    alert("Удалите товары из избранного, чтобы добавить новые!");
  }
}
function onSingleItemPick(e) {
  if (e.target.classList.contains("selected")) {
    pickedItem.value = null;
    return;
  }
  const id = parseInt(e.target.dataset.id);
  if (isNaN(id)) return;
  const item = singlePickItems.find((el) => el.id === id);
  if (!item) return;
  pickedItem.value = item;
}
function removePickedItem(e, single = false) {
  if (single && pickedItem.value) {
    pickedItem.value = null;
    return;
  }
  const id = parseInt(e.target.dataset.id);
  console.log(id, "id");
  if (isNaN(id)) return;
  pickedItemsArr.value = pickedItemsArr.value.filter((el) => el?.id !== id);
  const elementToRemove = document.querySelector(
    `.lower-block__segment [data-id='${id}']`
  );
  console.log(elementToRemove);
  if (elementToRemove && elementToRemove.classList.contains("selected")) {
    elementToRemove.classList.toggle("selected");
  }
}
</script>
<style scoped></style>
