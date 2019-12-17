<template>
  <div>
      <label v-if="label">{{ label }}</label>
      <input :value="value" @input="updateValue" v-bind="$attrs"/>
  </div>
</template>

<script>
export default {
  inheritAttrs: false,
  props: {
    label: {
      type: String,
      required: false,
      default: ''
    },
    value: [String, Number]
  },
  methods: {
    updateValue(event) {
      this.$emit('input', event.target.value)
    }
  }
}
</script>

<style>
</style>
