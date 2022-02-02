<template>
  <div class="card relative" :style="'background-color: ' + savedColor" :id="id" v-on:click="onSelect" :class="{'active': active}">
    <div class="closeCard" v-on:click="onClose">x</div>
    <input v-model="savedColor">
  </div>
</template>

<script>
export default {
  name: "Card",
  data: function () {
    return {
      savedColor: 'blue',
    }
  },
  props: {
    keyInTab: Number,
    id: Number,
    active: Boolean,
    color: String
  },
  watch: {
    savedColor: function () {
      this.$emit('colorChange', this.id, this.savedColor)
    },
    color: function () {
      this.savedColor = this.color
    }
  },
  created() {
    this.savedColor = this.color
  },
  methods: {
    onClose: function () {
      this.$emit('onCloseCard', this.id)
    },
    onSelect: function () {
      this.$emit('onSelect')
    }
  }
}
</script>

<style scoped>
  .card {
    background-color: blue;
    height: 100px;
    border-radius: 10px;
    display: flex;
    justify-content: center;
    align-items: flex-end;
    animation: 0.3s appear;
    margin-bottom: 10px;
    width: 100%;
  }

  @keyframes appear {
    0%   {opacity: 0;}
    25%  {opacity: 0.30;}
    50%  {opacity: 0.60;}
    75%  {opacity: 0.90;}
    100% {opacity: 1;}
  }

  .closeCard {
    position: absolute;
    top: 10px;
    right: 10px;
    color: white;
    cursor: pointer;
  }

  .card input {
    width: 70%;
    margin-bottom: 5%;
    text-align: center;
    text-transform: capitalize;
  }
</style>
