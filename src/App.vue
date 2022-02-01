<template>
  <div id="app">
    <h1>Test technique 2 de HeyTeam</h1>
    <div class="flex container justify-between">
      <CardContainer :cards="cardContainer1" @onActive="disableAllActive"/>
      <div class="button-container">
        <button v-on:click="onMove" :class="{activeButton : !isActiveCard || !canMove }">
          Move
        </button>
        <button v-on:click="onDelete" :class="{activeButton : !isActiveCard }">
          Delete
        </button>
        <button v-on:click="onCopy" :class="[{activeButton : !isActiveCard || !canCopy}]">
          Copy
        </button>
        <button v-on:click="onReference" :class="{activeButton : !isActiveCard || !canCopy}">
          Reference
        </button>
      </div>
      <CardContainer :cards="cardContainer2" @onActive="disableAllActive"/>
    </div>
  </div>
</template>

<script>
import CardContainer from './components/CardContainer.vue'
import Card from "@/components/Card";

export default {
  name: 'App',
  components: {
    CardContainer
  },
  data: function () {
    return {
      cardContainer1: [],
      cardContainer2: []
    }
  },
  methods: {
    disableAllActive: function () {
      this.cardContainer1.forEach(item=>item.isActive=false)
      this.cardContainer2.forEach(item=>item.isActive=false)
    },
    onMove: function () {
      if(!this.isActiveCard) {
        return
      }
      let cardToMove = null
      let isInContainer1 = false
      this.cardContainer1 = this.cardContainer1.filter(item=> {
        if(!item.isActive) {
          return true
        } else {
          if(this.cardContainer2.length<6) {
            isInContainer1 = true
            cardToMove = item
          } else {
            return true
          }
        }
      })
      this.cardContainer2 = this.cardContainer2.filter(item=> {
        if(!item.isActive) {
          return true
        } else {
          if(this.cardContainer1.length<6) {
            cardToMove = item
          } else {
            return true
          }
        }
      })
      if(isInContainer1) {
        if(this.cardContainer2.length<6) {
          this.cardContainer2.push(cardToMove)
        }
      } else {
        if (this.cardContainer1.length<6) {
          this.cardContainer1.push(cardToMove)
        }
      }
    },
    onDelete: function () {
      this.cardContainer1 = this.cardContainer1.filter(item=> {
        return !item.isActive
      })
      this.cardContainer2 = this.cardContainer2.filter(item=> {
        return !item.isActive
      })
    },
    onCopy: function () {
      this.cardContainer1.forEach(item=>{
        if(item.isActive && this.cardContainer1.length<6) {
          this.cardContainer1.push({
            id: Math.floor(Math.random() * Number.MAX_VALUE),
            component: Card,
            isActive: false,
            color: 'blue'
          })
        }
      })
      this.cardContainer2.forEach(item=>{
        if(item.isActive && this.cardContainer2.length<6) {
          this.cardContainer2.push({
            id: Math.floor(Math.random() * Number.MAX_VALUE),
            component: Card,
            isActive: false,
            color: 'blue'
          })
        }
      })
    },
    onReference: function () {
      this.cardContainer1.forEach(item=>{
        if(item.isActive && this.cardContainer1.length<6) {
          this.cardContainer1.push({
            id: Math.floor(Math.random() * Number.MAX_VALUE),
            component: Card,
            isActive: false,
            color: item.color
          })
        }
      })
      this.cardContainer2.forEach(item=>{
        if(item.isActive && this.cardContainer2.length<6) {
          this.cardContainer2.push({
            id: Math.floor(Math.random() * Number.MAX_VALUE),
            component: Card,
            isActive: false,
            color: item.color
          })
        }
      })
    },
  },
  computed: {
    isActiveCard: function () {
      let isElActive = false
      this.cardContainer1.forEach(item=>{
        if(item.isActive){
          isElActive = true
        }
      })
      this.cardContainer2.forEach(item=>{
        if(item.isActive){
          isElActive = true
        }
      })
      return isElActive
    },
    canCopy: function () {
      let canCopy = false
      this.cardContainer1.forEach(item=>{
        if(item.isActive && this.cardContainer1.length < 6){
          canCopy = true
        }
      })
      this.cardContainer2.forEach(item=>{
        if(item.isActive && this.cardContainer2.length < 6){
          canCopy = true
        }
      })
      return canCopy
    },
    canMove: function () {
      let canCopy = false
      this.cardContainer1.forEach(item=>{
        if(item.isActive && this.cardContainer2.length < 6){
          canCopy = true
        }
      })
      this.cardContainer2.forEach(item=>{
        if(item.isActive && this.cardContainer1.length < 6){
          canCopy = true
        }
      })
      return canCopy
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.flex {
  display: flex;
}

.relative {
  position: relative;
}

.container {
  width: 80%;
  margin: 0 auto;
}

.justify-between {
  justify-content: space-between;
}

.button-container {
  margin: 0 30px;
}

button {
  margin: 10px 0;
  width: 100%;
  padding: 10px;
  background-color: orange;
  border: none;
  border-radius: 5px;
  color: white;
  box-shadow: 2px 2px 4px 2px rgba(0, 0, 0, 0.2);
  cursor: pointer;
}

.activeButton {
  opacity: 0.4;
}
</style>
