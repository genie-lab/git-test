<template>
  <q-page-container>
    <div></div>
    <q-card>
      <q-card-section class="row text-center">
        <q-card-section class="col text-h4 text-primary">
          {{ header || "Welcome" }}
          <q-btn
            v-if="editing"
            @click="doEdit(false)"
            color="primary"
            unelevated
            rounded
            icon-right="clear"
            label="Cancel"
          ></q-btn>
          <q-btn
            v-else
            @click="doEdit(true)"
            color="primary"
            unelevated
            rounded
            icon-right="send"
            label="Add Item"
          ></q-btn>
        </q-card-section>
      </q-card-section>

      <q-card-section class="row justify-center">

        <q-card-section v-if="editing" class="col-8 col-md-3">
          <q-input
            v-model="newItem"
            @keyup.enter="saveItem"
            label="Add on Item"
          />
        </q-card-section>

        <q-card-section v-if="editing" class="col-4 col-md-3">
          <q-checkbox
            class="q-pa-none"
            :disable="newItem.length == 0 ? true : false"
            v-model="newItemHighPriority"
            :true-value="true"
            :false-value="false"
            label="High Priority"
          />
        </q-card-section>
        <q-card-section v-if="editing" class="col-12 col-md-3">
          <q-btn outline rounded color="primary" @click="saveItem(item)" label="Save item"></q-btn>
        </q-card-section>
      </q-card-section>

      <q-card-section>
        <q-list bordered>
          <q-item
            v-for="item in reversedItems"
            @click="togglePurchased(item)"
            :key="item.id"
            class="rounded-borders"
            :class="{ strikeout: item.purchased, priority: item.highPriority }"
            dense
            padding
            clickable v-ripple
          >
            <q-item-section >
              {{ item.label }}
            </q-item-section>
          </q-item>
        </q-list>
      </q-card-section>

      <q-item-section class="row text-center">
        <q-card-section>
          <span v-if="items.length === 0"
            >Nice job! You've bought all your items!</span
          >
        </q-card-section>
      </q-item-section>
    </q-card>
  </q-page-container>
</template>

<script>
export default {
  name: "vueBasic",

  data() {
    return {
      header: "Shopping List App",
      editing: false,
      newItem: "",
      newItemHighPriority: false,
      items: [
        { id: 1, label: "10 party hats", purchased: true, highPriority: false },
        { id: 2, label: "2 board games", purchased: true, highPriority: false },
        { id: 3, label: "20 cups", purchased: false, highPriority: true },
      ],
    };
  },

  computed: {
    reversedItems() {
      return [...this.items].reverse();
    },
  },
  watch() {},
  mounted() {},
  methods: {
    doEdit(editing) {
      this.editing = editing;
      this.newItem = "";
      this.newItemHighPriority = false;
    },

    async saveItem() {
      if (this.newItem.length == 0) return;
      this.items.push({
        id: this.items.length + 1,
        label: this.newItem,
        highPriority: this.newItemHighPriority,
      }),
        console.log(this.items);
      await this.$q.notify({
        color: "primary",
        icon: "done",
        message: ` <span style="color: white"><strong style="color: yellow">${this.newItem}</strong> 상품을 담았습니다</span>`,
        html: true,
      });
      this.newItem = "";
      this.newItemHighPriority = false;
    },

    togglePurchased(item) {
      item.purchased = !item.purchased;
    },
  },
};
</script>

<style lang="scss">
.strikeout {
  text-decoration: line-through;
  color: #b8c2cc;
}

.strikeout:hover {
  color: #8795a1;
}

.priority {
  color: #de751f;
}
</style>
