<template>
  <div class="select" :class="{ select_show: selectDropdown}">
    <button
      type="button"
      class="select__trigger"
      @click="selectDropdown = !selectDropdown"
    >
      {{ selectValue }}
    </button>
    <div v-if="selectDropdown" class="select__dropdown">
      <ul class="select__dropdown__list">
        <li
          v-for="item in items"
          :key="item.id"
          class="select__dropdown__item"
          @click="getSelectedItem(item)"
        >
          {{ item.name }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>

export default {
  props: {
    items: Array
  },
  data: () => ({
    selectDropdown: false,
    selectValue: "Язык"
  }),
  methods: {
    getSelectedItem(item) {
      this.selectDropdown = !this.selectDropdown
      this.selectValue = item.name
      this.$emit("update:selectValue", item.name)
    }
  }
}
</script>

<style scoped>

</style>