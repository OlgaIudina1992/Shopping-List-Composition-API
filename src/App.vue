<script setup>
import { ref, computed } from "vue";

const header = ref("Shopping List");
const shopping = ref([
  { id: 1, label: "bananas", purchased: true, itemPriority: false },
  { id: 2, label: "apples", purchased: false, itemPriority: true },
  { id: 3, label: "crickets", purchased: true, itemPriority: true },
  { id: 2, label: "pears", purchased: false, itemPriority: false}
]);
  const shoppingRev = computed(() => {
    return [...shopping.value].reverse()
  })
const newItem = ref("");
const charaCount = computed(() => {
  return newItem.value.length;
})

const addItem = () => {
  shopping.value.push({
    id: shopping.value.length + 1,
    label: newItem.value,
    itemPriority: itemPriority.value
  })
  newItem.value="";
  itemPriority.value="";
}
const empty = ref(false);
const itemPriority = ref(false);
const doAdd = (e) => {
  empty.value = e;
  newItem.value = "";
  itemPriority.value="";
}
const togglePurchase = (item) => {
  item.purchased = !item.purchased;
}
 </script>

<template>
  <body>
    <h1>{{ header || "Welcome" }}</h1>
     <div id="list" >
      <div class="button-list">
       <button v-if="empty" id="start-btn" @click="doAdd(false)">
        Let's not...
      </button>
         <button v-else id="start-btn" @click="doAdd(true)">
        Let's Shop!
      </button>
    </div>
      <form @submit.prevent="addItem" v-if="empty">
        <input
        @keyup.backspace="newItem.length - 1"
        v-model.trim="newItem"
        id="add"
        type="text"
        placeholder="Things to buy..."
        :disabled="newItem.length === 30"
        
      />
      <label>
        <input id="priority" value="high" type="checkbox" v-model="itemPriority" />
        Urgent
      </label>
      <br />
      <p class="counter">{{charaCount}}/30</p>
      <button id="selection" :disabled="newItem.length < 4">
        Save Item
      </button>
     
      <ul >
        <li v-for="item in shoppingRev" 
            :key="item.id" 
            :class="{
                    strikethru: item.purchased,
                    priority: item.itemPriority
                    }" 
            @click="togglePurchase(item)"
            >
          {{ item.label }} 
        </li>
      </ul>
      </form>
      <p v-if="!shopping.length" id="hiddenEl">Add an Item to Start</p>
    </div>
  </body>
</template>


