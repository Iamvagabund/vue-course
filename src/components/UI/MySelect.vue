<template>
  <select :value="modelValue" @change="changeOption">
    <option disabled value="">Выберите из списка</option>
    <option 
      v-for="option in options" 
      :key="options.value" 
      :value='options.value'
    >
      {{ options.name }}
    </option>
  </select>
</template>

<script>
  export default {
    name: 'MySelect',
    props: {
      modelValue: {
        type: String
      },
      options: {
        type: Array,
        default: () => []
      }
    },
    methods: {
      changeOption(value) {
        this.$emit('update:modelValue', value)
      }
    }
  }
</script>

<style scoped>

</style>