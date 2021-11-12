<template>
  <div class="container">
    <DropDown @toggleActive="setOpened" :opened="opened">
      <template #drop-dow-toggle>
        <MyInputSelect
          :value="selected"
          :validateList="validateList"
          :placeholder="label"
          errorText="Не вернный ввод"
          v-model="selected"
          @input="inputHandler"
          :label="label"
          @not-valid="$emit('not-valid')"
          @valid="$emit('valid')"
          @setActive="setActive"
        />
      </template>
      <template #drop-dow-content>
        <div class="options active" v-if="filteredItems.length">
          <div
            v-for="item in filteredItems"
            :key="item.id"
            :class="[
              'select-list-item',
              {
                'select-list-item_selected': item[propName] === selected,
              },
            ]"
            @click="selectHandler(item[propName])"
          >
            {{ item[propName] }}
          </div>
        </div>
        <div class="options active" v-else>
          <div class="not-found">Не найдено</div>
        </div>
      </template>
    </DropDown>
  </div>
</template>

<script>
import DropDown from "./DropDown";
import MyInputSelect from "./MyInputSelect";

export default {
  name: "MySelect",
  components: { MyInputSelect, DropDown },
  props: {
    items: {
      type: Array,
    },
    propName: {
      type: String,
      default: "name",
    },
    label: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      selected: null,
      active: false,
      validateList: [],
      opened: false,
    };
  },
  created() {
    this.validateList = this.items.map((item) => item[this.propName]);
  },
  computed: {
    filteredItems() {
      return this.selected
        ? this.items.filter((item) =>
            item[this.propName]
              .toLowerCase()
              .startsWith(this.selected.toLowerCase())
          )
        : this.items;
    },
  },
  methods: {
    setOpened(val) {
      this.opened = val || this.active;
    },
    setActive(val) {
      this.active = val;
    },
    selectHandler(item) {
      this.selected = item;
      this.$emit("input", this.selected);
      this.opened = false;
    },
    inputHandler() {
      this.$emit("input", this.selected);
    },
  },
};
</script>

<style scoped>
.container {
  position: relative;
}
.options {
  position: absolute;
  top: 34px;
  width: calc(100% - 14px);
  min-height: 30px;
  max-height: 100px;
  background: white;
  border-left: 2px solid dodgerblue;
  border-right: 2px solid dodgerblue;
  border-bottom: 2px solid dodgerblue;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
  display: none;
  z-index: 2;
}

.options::-webkit-scrollbar-thumb {
  background-color: dodgerblue;
  border: 4px solid transparent;
  border-radius: 8px;
  background-clip: padding-box;
  height: 30px;
}

.options::-webkit-scrollbar {
  width: 16px;
}

.options.active {
  display: block;
  margin-left: 7px;
  margin-right: 7px;
  overflow-x: hidden;
  overflow-y: auto;
  margin-bottom: 7px;
}
.not-found {
  color: black;
  height: 20px;
  padding: 0 15px;
  line-height: 20px;
}
.options .select-list-item {
  color: black;
  height: 20px;
  padding: 0 15px;
  line-height: 20px;
  cursor: pointer;
  overflow: hidden;
  text-overflow: ellipsis;
}
.select-list-item:hover {
  background: #8d95a5;
}

.options .select-list-item_selected {
  background: dodgerblue;
}
</style>
