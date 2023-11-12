<template>
  <main> 
    <div id="head">
      <img id="head-img" src="https://static.vecteezy.com/system/resources/previews/007/773/416/large_2x/dramatic-dark-sky-and-clouds-cloudy-sky-background-black-sky-before-thunder-storm-and-rain-background-for-death-sad-grieving-or-depression-free-photo.jpg">
      <h1>Welcome to The Cheesy Shack</h1>
    </div>
    <section id="burger-selection">
      <h2 id="subtitle">Fantastic Burgers and where to find them</h2>
      <Burger v-for="burger in burgers"
          v-bind:burger="burger" 
          v-bind:key="burger.name"/>
    </section>
            <section id="customer-information">
                <form>
                    <h2 id="muggle-title">Muggle information</h2>
                    <div id="muggle-info">
                        <p>
                            <label for="name">Full name</label><br>
                            <input type="text" id="name" name="n" required="required" placeholder="First- and lastname">
                        </p>
                        <p>
                            <label for="email">E-mail</label><br>
                            <input type="email" id="email" name="em" placeholder="E-mail address">
                        </p>
                        <p>
                            <label for="nr">Phone number</label><br>
                            <input type="tel" id="nr" name="nr" placeholder="Phone number">
                        </p>
                        <p>
                            <label for="street">Street</label><br>
                            <input type="text" id="street" name="st" required="required" placeholder="Street name">
                        </p>
                        <p>
                            <label for="housenr">House</label><br>
                            <input type="number" id="housenr" name="hnr" required="required" placeholder="House number">
                        </p>
                        <p>
                            <input type="radio" id="female" name="gender">
                            <label for="female">Witch</label>
                            <input type="radio" id="male" name="gender">
                            <label for="male">Wizard</label>
                            <input type="radio" id="other" name="gender">
                            <label for="other">Muggle</label>
                        </p>
                        <h3>Payment choices</h3>
                        <p>
                            <label for="currency">Currency</label>
                            <select id="currency" name="curr">
                                <option selected="selected">Galleons</option>
                                <option>Sickles</option>
                                <option>Knots</option>
                                <option>Muggle money</option>
                            </select>
                        </p>
                        <h4>Receipt via:</h4>
                        <p>
                            <input type="checkbox" id="owl" name="r">
                            <label for="owl">Owl</label>
                            <input type="checkbox" id="e_mail" name="r">
                            <label for="e_mail">E-mail</label>
                            <input type="checkbox" id="sms" name="r">
                            <label for="sms">SMS</label>
                        </p>
                    </div>
                </form>
            </section>
            <button type="submit">
                Place my order
                <img src="/img/wand.png" style="height: 30px">
            </button>
        </main>
        <footer>&copy; 2023 The Cheesy Shack</footer>
</template>

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
      burgers: menu
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
#burger-layout, #customer-information, button {
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
.burger-layout {
    display: grid;
    grid-template-columns: 21em 21em 21em;
    grid-gap: 1em;
    margin: 1em 4em 2em 4em;
}

.burger_1 {
    grid-row: 1;
    grid-column: 1;
}

.burger_2 {
    grid-row: 1;
    grid-column: 2;
}

.burger_3 {
    grid-row: 1;
    grid-column: 3;
}

.burger_4 {
    grid-row: 2;
    grid-column: 1;
}

.burger_5 {
    grid-row: 2;
    grid-column: 2;
}

.burger_6 {
    grid-row: 2;
    grid-column: 3;
}



/*Styling of customer information*/

#customer-information {
    width: 33.5em;
    background-color:  rgb(184, 205, 198);
    border: 5px solid black;
}
#muggle-title {
    background-color: rgb(40, 68, 69);
    margin: 0em;
    padding: 1em 1em 1em 2em;
    color: goldenrod;
    border-bottom: 10px solid black;
}
#muggle-info {
    padding: 0em 1em 0em 3em;
}
button {
    margin-top: 2em;
    margin-bottom: 2em;
    margin-left: 8em;
    padding: 0.5em 0.5em 0.5em 0.5em;
    background-color: goldenrod;
    border: 4px solid rgb(160, 122, 25);
    font-size: 1em;
    font-family: "Harry Potter", serif;
}

button:hover {
    background-color: gold;
 }

 footer {
    background-color: rgb(27, 46, 47);
    margin: 0em;
    padding: 0.5em 1em 2em 1em;
    color: goldenrod;
    border-top: 0.4em solid black;
 }
</style>