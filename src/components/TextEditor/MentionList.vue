<template>
  <div>
    <div
      v-if="items.length"
      class="min-w-40 rounded-lg border bg-surface-white p-1 text-base shadow-lg"
    >
      <button
        :class="[
          index === selectedIndex ? 'bg-surface-gray-2' : '',
          'flex w-full items-center whitespace-nowrap rounded-md px-2 py-2 text-sm text-ink-gray-9',
        ]"
        v-for="(item, index) in items"
        :key="index"
        @click="selectItem(index)"
        @mouseover="selectedIndex = index"
      >
        {{ item.label }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
      required: true,
    },
    command: {
      type: Function,
      required: true,
    },
  },
  data() {
    return {
      selectedIndex: 0,
    }
  },
  watch: {
    items() {
      this.selectedIndex = 0
    },
  },
  methods: {
    onKeyDown({ event }) {
      if (event.key === 'ArrowUp') {
        this.upHandler()
        return true
      }
      if (event.key === 'ArrowDown') {
        this.downHandler()
        return true
      }
      if (event.key === 'Enter') {
        this.enterHandler()
        return true
      }
      return false
    },
    upHandler() {
      this.selectedIndex =
        (this.selectedIndex + this.items.length - 1) % this.items.length
    },
    downHandler() {
      this.selectedIndex = (this.selectedIndex + 1) % this.items.length
    },
    enterHandler() {
      this.selectItem(this.selectedIndex)
    },
    selectItem(index) {
      const item = this.items[index]
      if (item) {
        this.command({ id: item.value, label: item.label })
      }
    },
  },
}
</script>

<style>
.item {
  display: block;
  margin: 0;
  width: 100%;
  text-align: left;
  background: transparent;
  border-radius: 0.4rem;
  border: 1px solid transparent;
  padding: 0.2rem 0.4rem;
}
.item.is-selected {
  border-color: #000;
}
</style>
