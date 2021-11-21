<template>
  <div>
    Burgers
    <Burger v-for="burger in burgers"
            v-bind:burger="burger" 
            v-bind:key="burger.name"/>
  </div>
  <div id="map" v-on:click="addOrder">
    click here
  </div>
</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'

const socket = io();

//Object Constructor Function

function MenuItem(burger, Kcal, url, lactose, gluten) {
 this.burgername = burger; // The *this* keyword refers to the object itself
 this.calories = Kcal;
this.pic = url;
 this.lactose = lactose;
this.gluten = gluten;
//  this.name = function() {
//  return this.firstName + ' ' + this.Kcal+  ' ' + this.url + ' ' + this.lactose+ ' ' +this.gluten;
//  };

}

const firstBurger = new MenuItem("Healthy", 350, "pic", false, true);
const SecondBurger = new MenuItem("Vegan", 500, "pic", false, false);
const ThirdBurger = new MenuItem("Maximized", 650, "pic", true, true);

console.log( firstBurger );


const burgers= [firstBurger, SecondBurger, ThirdBurger];
console.log( burgers );



export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: firstBurger
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
