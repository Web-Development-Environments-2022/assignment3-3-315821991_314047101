<template>
  <div id="app">
    <b-button @click="showModal" ref="btnShow">Open</b-button>
    <b-button @click="toggleModal" ref="btnToggle">Toggle</b-button>

    <b-modal id="modal">
      <div class="d-block">Hello</div>
      <b-button @click="hideModal">Close</b-button>
      <b-button @click="toggleModal">Toggle</b-button>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: "App",
  methods: {
    showModal() {
      this.$root.$emit("bv::show::modal", "modal", "#btnShow");
    },
    hideModal() {
      this.$root.$emit("bv::hide::modal", "modal", "#btnShow");
    },
    toggleModal() {
      this.$root.$emit("bv::toggle::modal", "modal", "#btnToggle");
    }
  }
};
</script>