<script setup>
</script>

<template>
     <button>
        <slot>Crea</slot>
     </button>
</template>

<style lang="scss"  scoped>
  button {
    padding: 8px 16px;
    border: none;
    cursor: pointer;
  }
</style>