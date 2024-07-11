<script>
export default {
  props: {
    changeAmount: {
      type: Function,
      required: true
    },
    convert: {
      type: Function,
      required: true
    }
  }
}
</script>

<template>
  <input
    min="0"
    type="number"
    placeholder="Введите число"
    @input="changeAmount($event.target.value)"
  />
</template>

<style scoped></style>
