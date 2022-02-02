<template>
  <div class = "cardContainer relative">
    <div class = "addButton" v-on:click = "addCard" :class = "{activeButton : cards.length>=6}">+</div>
    <div class = "flex">
      <component v-for = "card in getCards" :is = "card.subCard.component" :id = "card.subCard.id" @onCloseCard = "onCloseCard"
                 :key = "card.id" class = "card-flex"
                 @onSelect = "changeStateCard(card)" :active = "card.isActive" :color = "card.subCard.color"
                 @colorChange = "changeColor"></component>
    </div>
  </div>
</template>

<script>
import Card from "@/components/Card";

export default {
  name: 'CardContainer',
  components: {Card},
  props: {
    cards: Array
  },
  methods: {
    addCard() {
      if (this.cards.length < 6) {
        let subCard = {
          id: Math.floor(Math.random() * Number.MAX_VALUE),
          component: Card,
          color: 'blue'
        }
        this.cards.push({
          isActive: false,
          subCard: subCard
        })
      }
    },
    onCloseCard(value) {
      this.cards.forEach((item, key) => {
        if (item.subCard.id === value) {
          this.cards.splice(key, 1)
        }
      })
    },
    changeStateCard(card) {
      this.$emit('onActive')
      card.isActive = !card.isActive
    },
    changeColor(value, color) {
      this.cards.forEach((item) => {
        if (item.subCard.id === value) {
          item.subCard.color = color
        }
      })
    }
  },
  computed: {
    getCards: function () {
      return this.cards
    }
  }
}
</script>

<style scoped>
.card-flex {
  width: 30%;
  margin-left:10px;
}

.cardContainer {
  background-color: #2c3e50;
  width: 100%;
  height: 70vh;
  padding: 5%;
}

.addButton {
  color: white;
  background-color: orange;
  width: 40px;
  height: 40px;
  border-radius: 20px;
  position: absolute;
  top: -20px;
  right: -20px;
  font-size: 25px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.flex {
  margin-left: -10px ;
  flex-wrap: wrap;
}

.active.card {
  border: 2px white solid;
}
</style>
