<template>
 <div id = "burger"  style="float:left;  ">
   <h3>{{burger.name}}</h3>
   <img v-bind:src="burger.img" alt="" style="margin-right: 15px; height: 300px; width: 300px" >
    <ul>
      <li> {{burger.kCal}} kCal</li>
      <li v-if="burger.gluten"> Contains <span class="Gluten">Gluten</span></li>
      <li v-if="burger.lactose">Contains <span class="Lactose">Lactose</span></li>
      </ul>
  <div>
    <button v-on:click="AddABurger">
        +
      </button>
      <button v-on:click="RemoveABurger">
        -
      </button>
      <p>amount: {{ amountOrdered }}</p>
  </div>
  </div>
            
</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  },

data: function () {
  return {
    amountOrdered: 0,
  
  }
 },
 
 methods: {
   AddABurger: function () {
     this.amountOrdered++
     this.$emit('orderedBurger', {
       name: this.burger.name,
       amount: this.amountOrdered
          }
      );
    },
    
    RemoveABurger: function () {
      this.amountOrdered--
      this.$emit('orderedBurger', {
        name: this.burger.name,
        amount: this.amountOrdered
          }
      );
    }
  }
}

      
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>



.Lactose {
    font-weight: bold;
}

.Gluten {
    font-weight: bold;

}

</style>