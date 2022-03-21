<template>
<div>
  <input type="text" placeholder="Search crypto">
  <select class="app-select" @input="onSelect">
    <option value="A">AAA</option>
    <option value="B">AAB</option>
    <option value="C">ABC</option>
  </select>
</div>
</template>

<script>
export default {
  name: 'AppSelect',

  methods: {
    onSelect (event) {
      console.log(event.target.value)
    }
  }
}
</script>

<style scoped>
* {
  text-align: center;
  background-color: #000;
  font-size: 1rem;
}
</style>
