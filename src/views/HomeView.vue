<script>
import Burger from '../components/OneBurger.vue'
import menu from '../assets/menu.json'
import io from 'socket.io-client'

const socket = io();




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
        burgers: menu,
        fullName: "",
        emailAddress: "",
        phoneNumber: "",
        gender: "witch",
        currency: "",
        reciept: [],
        BurgerOrders: {},
        location: { x: 0, y: 0}
    }
  },
  methods: {
    recieveAddedBurger(order) {
        this.BurgerOrders[order.name] = order.amount;
        console.log(this.BurgerOrders)
    },
    recieveRemovedBurger(order) {
        if (this.BurgerOrders[order.name] === 1) {
            delete this.BurgerOrders[order.name]
        } else {
            this.BurgerOrders[order.name] = order.amount;
        }
        console.log(this.BurgerOrders)

    },
    printInConsole: function() {
        console.log(this.fullName, this.emailAddress, this.phoneNumber, this.gender, this.currency, this.reciept);
    },
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

<template>
    <main> 
      <div id="head">
        <img id="head-img" src="https://static.vecteezy.com/system/resources/previews/007/773/416/large_2x/dramatic-dark-sky-and-clouds-cloudy-sky-background-black-sky-before-thunder-storm-and-rain-background-for-death-sad-grieving-or-depression-free-photo.jpg">
        <h1>Welcome to The Cheesy Shack</h1>
      </div>
      <section id="burger-selection">
        <h2 id="subtitle">Fantastic Burgers and where to find them</h2>
        <div id="burger-container">
          <Burger 
              v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"
              v-on:addBurger="recieveAddedBurger($event)"
              v-on:removeBurger="recieveRemovedBurger($event)">
          </Burger>
        </div>
      </section>
        <section id="customer-information">
            <div id="info-1">
                    <form>
                      <h2 class="info-title">Muggle information</h2>
                      <div id="muggle-info">
                          <p>
                              Full name<br>
                              <input v-model="fullName" type="text" placeholder="First- and lastname"> 
                          </p>
                          <p>
                              E-mail<br>
                              <input v-model="emailAddress" type="email" placeholder="E-mail address">
                          </p>
                          <p>
                              Phone number<br>
                              <input v-model="phoneNumber" type="tel" placeholder="Phone number">
                          </p>
                          <p>
                              <input v-model="gender" type="radio" required="required" id="witch" value="witch">
                              <label for="witch">Witch</label>
                              <input v-model="gender" type="radio" id="wizard" value="wizard">
                              <label for="wizard">Wizard</label>
                              <input v-model="gender" type="radio" id="muggle" value="muggle">
                              <label for="muggle">Muggle</label>
                          </p>
                          <h3>Payment choices</h3>
                          <p>
                              <label for="currency">Currency </label>
                              <select v-model="currency" id="currency" required="required">
                                  <option disabled value="">Please select one</option>
                                  <option>Galleons</option>
                                  <option>Sickles</option>
                                  <option>Knots</option>
                                  <option>Muggle money</option>
                              </select>
                          </p>
                          <h4>Receipt via:</h4>
                          <p>
                              <input v-model="reciept" type="checkbox" value="owl" id="owl">
                              <label for="owl">Owl</label>
                              <input v-model="reciept" type="checkbox" value="email" id="e_mail">
                              <label for="e_mail">E-mail</label>
                              <input v-model="reciept" type="checkbox" value="sms" id="sms">
                              <label for="sms">SMS</label>
                          </p>
                      </div>
                  </form>

                  </div>
                  <div id="info-2">
                    <h2 class="info-title">Location of delivery</h2>
                    <h3 style="margin-left: 2em;">Click on the map where the order shall be delivered</h3>
                    <div id="location" v-on:click="addOrder" v-bind:style="{ left: this.location.x + 'px', 
                      top: this.location.y + 'px' }">
                      <p id="dot">
                        X
                      </p>
                        <img id="map" src="/img/polacks.jpg">
                    </div>
                  </div>
              </section>
              <button v-on:click="printInConsole()" type="submit" id="order-button">
                  Place my order
                  <img src="/img/wand.png" style="height: 30px">
              </button>
          </main>
          <footer>&copy; 2023 The Cheesy Shack</footer>
  </template>
  

<style>
@import url('https://fonts.cdnfonts.com/css/harry-potter');
@import url('https://fonts.googleapis.com/css2?family=EB+Garamond&family=Inconsolata&family=Noto+Serif&family=Roboto+Condensed&family=Roboto:wght@100&display=swap');

/*Standard font and background color*/
body {
    font-family: "EB Garamond", serif;
    font-size: 1em;
    background-color: rgb(108, 169, 155);
    margin: 0em;
}

/*Margin of indented content*/
#customer-information, button {
    margin-left: 4em;
    margin-right: 4em;
}

/*Styling of the head of the page*/
#head {
    position: relative;
}

h1 {
    position: absolute;
    top: 40%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: center;
    font-size: 3em;
    color: goldenrod;
    font-family: "Harry Potter", serif;
}

#head-img {
    display: block;
    opacity: 0.7;
    object-fit: cover;
    width: 100%;
    height: 15em;
}

#subtitle {
    background-color: rgb(27, 46, 47);
    border-top: 0.4em solid black;
    margin:0em;
    padding: 1em 1em 1em 3em;
    color: goldenrod;
    font-size: 1.7em;
}

/*Layout of burger options*/
#burger-container {
    display: grid;
    grid-template-columns: 21em 21em 21em;
    grid-gap: 1em;
    margin: 1em 4em 2em 4em;
}

/*Styling of customer information*/
#customer-information {
    display: grid;
    grid-template-columns: 21em 43.2em;
    grid-template-rows: 34.5em;
    grid-gap: 1em;
}

#info-1 {
    grid-row: 1;
    grid-column: 1;
}

#info-2 {
    grid-row: 1;
    grid-column: 2;
    background-color: red;
    border: 5px solid black;
    background-color:  rgb(184, 205, 198);
}

form {
    width: 20.7em;
    background-color:  rgb(184, 205, 198);
    border: 5px solid black;
}
.info-title {
    background-color: rgb(40, 68, 69);
    margin: 0em;
    padding: 1em 1em 1em 2em;
    color: goldenrod;
    border-bottom: 10px solid black;
}
#muggle-info {
    padding: 0em 1em 0em 3em;
}

#location {
    overflow: scroll;
    height: 24.5em;
    position: relative;
}
#map {
    height: auto;
}
#dot {
    position: absolute;
    padding: 0em;
    margin: 0em;
}
/*Styling of order button*/
#order-button {
    margin-top: 2em;
    margin-bottom: 2em;
    margin-left: 8em;
    padding: 0.5em 0.5em 0.5em 0.5em;
    background-color: goldenrod;
    border: 4px solid rgb(160, 122, 25);
    font-size: 1em;
    font-family: "Harry Potter", serif;
}

#order-button:hover {
    background-color: gold;
 }

/*Styling of footer */
 footer {
    background-color: rgb(27, 46, 47);
    margin: 0em;
    padding: 0.5em 1em 2em 1em;
    color: goldenrod;
    border-top: 0.4em solid black;
 }

</style>