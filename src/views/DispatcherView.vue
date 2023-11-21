<template>
  <div id="orders">
    <div id="orderList">
      <div v-for="(order, key) in orders" v-bind:key="'order'+key">
        <p id = "orderInfo">
          #{{ key }}: {{ formattingOrder(order.orderItems) }}
        </p>
        <p id="customerInfo">
          {{ formattingCustInfo(order.customerInfo) }}
        </p>
        <hr>
      </div>
        <button v-on:click="clearQueue">Clear Queue</button>
      </div>
      <div class="wrapper">
        <div id="dots" v-bind:style="{ background: 'url(' + require('../../public/img/polacks.jpg')+ ')' }">
            <div v-for="(order, key) in orders" v-bind:style="{ left: order.details.x + 'px', top: order.details.y + 'px'}" v-bind:key="'dots' + key">
              {{ key }}
            </div>
        </div>
    </div>
    </div>
  </template>
  <script>
  import io from 'socket.io-client'
  const socket = io();
  
  export default {
    name: 'DispatcherView',
    data: function () {
      return {
        orders: null
      }
    },
    created: function () {
      socket.on('currentQueue', data =>
        this.orders = data.orders);
        console.log(this.orders)
    },
    methods: {
      clearQueue: function () {
        socket.emit('clearQueue');
      },
      formattingOrder: function (items) {
        let keysArr = Object.keys(items);
        let valArr = Object.values(items);
        let res = ""
        for (let i in keysArr) {
          res += keysArr[i] + ': ' + valArr[i] + ', ';
        }
        return res.slice(0,-2)
      },
      formattingCustInfo: function (info) {
        let valArr = Object.values(info);
        let res = ""
        res += valArr[0] + ' ('
        for (let i = 1; i < valArr.length-1; i += 1) {
          res += valArr[i] + ', ';
        }
        res = res.slice(0,-2);
        res += '), ' + 'Reciept via: ' + valArr.slice(-1);
        return res
      }
    }
  }
  </script>
  <style>
  #orderList {
    top:1em;
    left:1em;
    position: absolute;
    z-index: 2;
    color:black;
    background: rgba(255,255,255, 0.5);
    padding: 1em;
  }
  #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }
  
  #dots div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }

  #orderInfo {
    margin:0em;
    padding:0em;
    font-size: 1.1em;
  }
  
  #customerInfo {
    margin: 0em;
    padding: 0em;
    font-family: italic;
  }
  </style>
  