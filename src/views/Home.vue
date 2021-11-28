<template>
<header id="header" >

   <img src="https://www.spectos.com/wp-content/uploads/2019/10/banner-hospitality-qualit%C3%A4tsmonitoring-hotel-gastronomie-event-1.jpg" alt= "">
   <h1> Welcome to BurgerOnline </h1>
</header>

<main>

    <section id="burgers">

      <h2>Select burger</h2>
      This is where you execute burger selection
      <div class="wrapper">
        
        <Burger v-for="burger in burgers"
            v-bind:burger="burger"
            v-bind:key="burger.name"
            v-on:orderedBurger="addToOrder($event)"/>
  
       <!--     <header style="color:white;">
                This is where you execute burger selection
            </header>
            <div class="box a">

                <header>
                    <h1>Healthy</h1>
                </header>
                <img src="https://i.dietdoctor.com/se/wp-content/2016/12/Meal-Delifresh-Low-Carb-1.jpg?auto=compress%2Cformat&w=1600&h=1032&fit=crop" alt="Span" title="Another in-line element" style="width: 150px;">

                <ul>

                    <li> Free from <span class="gluten">Gluten</span></li>
                    <li>Contains <span class="lactose">Lactose</span></li>

                </ul>
            </div>


            <div class="box b">

                <header>
                    <h1>Vegan</h1>
                </header>
                <img src="https://www.korvbrodsbagarn.se/siteassets/receptbilder/brookyn_vegan_burger_recept_1170x790px.jpg" alt="Span" title="Another in-line element" style="width: 150px;">

                <ul>

                    <li> Free from <span class="lactose">Lactose</span></li>
                    <li>Contains <span class="gluten">Gluten</span></li>

                </ul>

            </div>

            <div class="box c">

                <header>
                    <h1>Maximized</h1>
                </header>
                <img src="https://kurera.se/wp-content/uploads/2015/09/bigmac_460x330.jpg" alt="Span" title="Another in-line element" style="width: 150px;">

                <ul>


                    <li>Contains <span class="gluten">Gluten</span></li>
                    <li>Contains <span class="everything">Everything</span></li>

                </ul>
            </div>
        </div>

        <div style="Clear:left">
            <header>
                <h1>  </h1>
            </header>
            -->

    </div>
   
    </section>
     
    

    <section id="customers" style="clear:left">
      <header>
        <h2 >Customer information </h2>
      </header>
      <div id="contact">
         This is where you provide necessary information
         <form>
           <p>
             <label for="fullname">Full name</label><br>
             <input type="text" id="fullname" v-model="na" required="required" placeholder="Fullname">
          </p>
          <p>
             <label for="email">E-mail</label><br>
             <input type="email" id="email" v-model="em" required="required" placeholder="E-mail-adress">
          </p>
          </form> 
      </div> 
      
       <!--  <p>
            <label for="Street">Street</label><br>
            <input type="Street" id="Street" v-model="st" required="required" placeholder="Street name">
        </p>

        <p>
            <label for="House">House</label><br>
            <input type="number" id="House" v-model="hn" required="required" placeholder="House number">
        </p>

-->    
<p>
  <label for="payment options">Payment options</label>
  <br>
  <select v-model="paym" >
     <option>Credit card</option>
     <option>Klarna</option>
     <option>Swish</option>
     <option>Apple Pay</option>
     <option>Collector</option>
  </select>
</p>
<header>
  Please indicate point of delivery:
  </header>
    
   <div id="scrollan">
     <div id="map" v-on:click="setLocation" >
       <div id="dots">
         <div v-bind:style="{ left: location.x + 'px', top: location.y + 'px'}">
           T
         </div>
       </div> 
     </div> 
    </div> 
     <section>
       <form>
         <p>Gender </p>
         <p>
           <input type="radio" id="Male" v-model="Gender" value="Male">
           <label for="Male">Male</label> <br>
       
           <input type="radio" id="Female" v-model="Gender" value="Female">
           <label for="Female">Female</label> <br>
       
           <input type="radio" id="Non-binary" v-model="Gender" value="Non-binary">
           <label for="Non-binary">Non-binary</label> <br>
       
           <input type="radio" id="Undisclosed" v-model="Gender" value="Undisclosed" checked >
           <label for="Undisclosed">Undisclosed</label> <br>
          </p>
       </form> 
    </section> 
    </section>

    <button v-on:click="clicksubmit">
        <img src="https://arbetsmedlarna.se/wp-content/uploads/2020/12/check-1.png" style="width: 20px">
          Place my order!
    </button>

   
  
</main>
<hr>

<footer style="margin-top:10px;">
    &copy; 2021 Hamburgers online Inc.
</footer>

  
  
</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

//Object Constructor Function

/*function MenuItem(name, kCal, img, lactose, gluten) {
 this.name = name; // The *this* keyword refers to the object itself
 this.kCal = kCal;
this.img = img;
 this.lactose = lactose;
this.gluten = gluten;
  }


*/
/*
let firstBurger = new MenuItem("Healthy", 350, "img", false, true);
let SecondBurger = new MenuItem("Vegan", 500, "img", false, false);
let ThirdBurger = new MenuItem("Maximized", 650, "img", true, true);
*/

const burgerss= menu;
console.log(burgerss);

export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
   return {
      burgers: burgerss,
      na: "",
      em: "",
      Gender: 'Undisclosed',
      payopt: 'Credit card',
      orderedBurgers:{},
      location: {
        x: 0,
        y: 0
    }
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
   /* addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    },*/
   
        clicksubmit: function () {
      
         socket.emit("addOrder", {
              orderId: this.getOrderNumber(),
              details: {
                x: this.location.x,
                y: this.location.y,
              },
              orderItem: this.orderedBurgers,
          form:{na: this.na, em: this.em, paym: this.paym, Gender: this.Gender}
            }
        )
        },
              
           
 
   addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    }, 

     setLocation: function(event) {
      let offset = {x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top};
      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 10 - offset.y;
    },
     buttonid: function() {
        console.log(this.orderedBurgers)
        socket.emit("addOrder", {
          orderId: this.getOrderNumber(),
          details: { x: this.location.x,
                y: this.location.y,
            
          },
          orderItems: [this.orderedBurgers]
        });
      },
    
  }
}

</script>

<style>

 
  /* @import url("reset.css");*/
/* @import url("https://fonts.googleapis.com/css?family=Droid+Serif|Share+Tech+Mono");*/
@import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';
body {
    font-family: "Times new roman";
    font-size: 12pt;
}

a, strong, em {
    color: #0099ff;
}

p.info,
#login p {
    width: 80%;
    margin: 1vw auto;
}


#header{
     margin-left: 10px;

     margin-right: 10px;
    overflow:hidden;
    height: 370px;
    
            }
#burgers {
  color: white;
  background-color: black;
  border: 2px dashed white;
  padding-left: 10px;
  margin-top: 10px;
  margin-left: 10px;
  margin-right: 10px;


}

#header img    {
opacity:0.5;
    width:100%;
    height: auto;
}

#dots {
  position: relative;
  margin: 0;
  padding: 0;
  background: url(/img/polacks.jpg);
  background-repeat: no-repeat;
  width:1920px;
  height: 1078px;
  cursor: crosshair;
}
input[type="radio"] {
    height: .9em;
    width: .9em;
  }

#header h1 {
     position: absolute;
     margin-top:-200px;
     padding-left: 10px;
     font-size: 40pt;
     color: black;
    }



#customers {
    padding-left: 10px;
    margin-top: 10px;
    margin-left: 10px;
    margin-right: 10px;
    margin-bottom: 10px;
    border: 2px dashed black;
    color: black;
    
    background-color: white;
}
 #map {
    width: 1920px;
    height: 1078px;
    padding-left: 40px;
    margin-top: 50px;
    margin-left: 50px;
    margin-right: 50px;
    margin-bottom:50px;
    background: url("/img/polacks.jpg");


  }
  #scrollan{
  width: 950px;
    height: 450px;
  overflow: scroll;
  }

.wrapper {
  display: grid;
  grid-template-columns: auto auto auto;
  
}

/*

.b {
    float:left;
    padding: 5px;
    margin-left:40px;
    grid-column: 3 ;
    grid-row: 1 / span 2;
   }


.c {
    float:left;
    padding: 5px;
    margin-left:40px;
    grid-column: 1 ;
    grid-row: 2 ;
}
*/

/*Button*/
button:hover {

background-color: pink;
   cursor:pointer;
}

#buttonid{
 margin-top: 20px;
 padding-left: 15px;
 margin-bottom: 15px
}
.button {
    margin-top:20px;
    margin-bottom:20px;
  }

</style>
