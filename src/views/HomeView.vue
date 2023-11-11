<template>
  <div>
    <div>
      <h1>Burgers</h1>
      <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"/>
    </div>
    <div id="map" v-on:click="addOrder">
      click here
    </div>
  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io();

let burgers = [{name:"The Whomping Whopper", img:"../../public/img/whomping_whopper.jpg", ingredients: ["Chili sauce", "Jalapeño", "Lettuce", "Onion"], lactose: true, gluten: true },
               {name:"The QuidCheese", img:"../../public/img/quidcheese.jpg", ingredients:["Cheddar", "Gouda", "Dressing", "Onion"], lactose: true, gluten: true},
               {name:"The Veggietaserum", img:"../../public/img/veggietaserum.jpg", ingredients:["Grilloumi", "Mayonnaise", "Tomato", "Lettuce"], lactose: true, gluten: true},
               {name:"The Hogsmeat", img:"../../public/img/hogsmeat.jpg", ingredients:["Cheddar", "Bacon", "Mayonnaise", "Onion"], lactose: true, gluten: true},
               {name:"The Marauders Smash", img:"../../public/img/marauders.jpg", ingredients:["Cheddar", "Dressing", "Lettuce", "Onion"], lactose: true, gluten: true},
               {name:"The Buckbeef", img:"../../public/img/buckbeef.jpg", ingredients:["Cheddar", "Dressing", "Onion"], lactose: true, gluten: true}]

function MenuItem (name, img, ingredients, lactose, gluten) {
  this.name = name;
  this.img = img;
  this.ingredients = ingredients;
  this.lactose = lactose;
  this.gluten = gluten;
}

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: burgers
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    }
  }
}
</script>

<style>
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
</style>