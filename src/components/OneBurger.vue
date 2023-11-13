<template>
  <div class="burger">
    <h3 class="burger-name">{{burger.name}}</h3>
    <img v-bind:src="burger.img" style="width: 300px" alt={{burger.img}}>
    <ul class="burger-info">
      <li>{{burger.ingredients}}</li>
        <section class="allergies">
          <li v-if="burger.gluten">Contains gluten</li>
          <li v-if="burger.lactose">Contains lactose</li>
        </section>
    </ul>
    <div id="button-container">
      <button class="amount-button">-</button>
      {{amountOrdered}}
      <button v-on:click="increaseAmount" class="amount-button">+</button>
    </div>
  </div>
</template>

  
<script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },

  data: function () {
    return {
      amountOrdered: 0,
    }
  },

  methods: {
    increaseAmount: function () {
      amountOrdered += 1;
    }
  }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  .burger {
    text-align: center;
    width: 20rem;
    height: 30em;
    border: 10px double rgb(44, 74, 76);
    background-color: white;
    }

  .burger-name {
    font-size: 1.5em;
    }

  #burger h3, #burger img {
    text-align: center;
    }

  .burger-info {
    text-align: left;
    margin-left: 3em;
    margin-right: 3em;
  }

  .allergies {
    color: darkred;
    text-transform: uppercase;
    padding-top: 1em;
  }

  #button-container {
    display: flex;
    justify-content: center;
  }
  .amount-button {
    background-color: goldenrod;
    border: 2px solid rgb(160, 122, 25);
    margin: 0em 2em 0em 2em;
  }
  .amount-button:hover {
    background-color: gold;
  }
  </style>
  