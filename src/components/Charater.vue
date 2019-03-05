<template>
  <div
    class="column is-12-mobile is-4-desktop is-4-tabet"
    v-bind:key="character.id"
  >
    <div class="card">
      <div class="card-header"><img :src="character.image" alt="" /></div>
      <div class="card-content">
        <h3 class="title is-size-4">{{ character.name }}</h3>
        <button
          class="button is-success is-rounded is-small"
          @click="verMas(character.id);"
        >
          Ver mas
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["character"],

  methods: {
    verMas(id) {
      this.$emit("showModal", id);
    }
  }
};
</script>
