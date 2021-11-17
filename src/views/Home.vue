<template>

  <div id="map" v-on:click="addOrder">
    click here
  </div>

  <header>
    <img class = "Title" id="TitleImage" src="https://thumbs.dreamstime.com/b/brick-wall-wide-panorama-masonry-wall-small-bricks-brick-wall-wide-panorama-gray-masonry-wall-small-bricks-modern-150754887.jpg">
    <h1 class = "Title">Welcome to the Burger Wall</h1>
  </header>
  <main>
    <section id="BurgerSelect">
      <h2>Pick your poison</h2>
      <p>Our burgers are packed with everything a burger should have; fat, carbs and fantastic taste!</p>
      <!-- The different burgers -->
      <div class="wrapper">
        <Burger v-for="burger in burgers"
              v-bind:burger="burger"
              v-bind:key="burger.name"/>
      </div>
    </section>

    <section id="CustomerInfo">
      <h2>Customer Information </h2>
      <p>Please provide the following information</p>
      <p>
          <label for="Name">Full Name</label><br>
          <input type="text" id="Name" v-model="fn" required="required" placeholder="First- and Last name">
      </p>
      <p>
          <label for="Email">E-mail Address</label><br>
          <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
      </p>
      <p>
          <label for="Street Name">Street Name</label><br>
          <input type="text" id="Street Name" v-model="sn" required="required" placeholder="Street name">
      </p>
      <p>
          <label for="House Number">House Number</label><br>
          <input type="text" id="House Number" v-model="hn" required="required" placeholder="House number">
      </p>
      <p>
          <label for="payment">Payment method</label><br>
          <select id="payment" v-model="pmt">
            <option>Debit Card</option>
            <option>Swish</option>
            <option>Invoice (Klarna)</option>
          </select>
      </p>
      <p>
          <label for="Gender">Gender</label><br>
          <input type="radio" id="female" v-model="gr" required="required" value="Female">
          <label for="female">Female</label><br>
          <input type="radio" id="male" v-model="gr" required="required" value="Male">
          <label for="male">Male</label><br>
          <input type="radio" id="nonbin" v-model="gr" required="required" value="Non-binary">
          <label for="nonbin">Non-binary</label><br>
          <input type="radio" id="undisclose" v-model="gr" required="required" value="Undisclosed">
          <label for="undisclosed">Undisclosed</label><br>
      </p>
    </section>
  </main>
  <button type="submit" v-on:click="placeOrder">
      <img src="https://cdn-icons-png.flaticon.com/512/660/660619.png" style = "height:1.5em;">
  </button>
  <footer>
    <!--To add special signs, use &code;-->
    <p>&copy; 2021 Burger Wall Inc.</p>
  </footer>

</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

console.log(menu);

const socket = io();

/*function MenuItem (prodName, imgURL, kCal, cont, fact) {
  this.name = prodName;
  this.imageURL = imgURL;
  this.calories = kCal;
  this.content = cont;
  this.descript = fact;
}

const burgerSelection = [new MenuItem("The Burning Burger", "https://www.kitchensanctuary.com/wp-content/uploads/2021/05/Double-Cheeseburger-square-FS-42.jpg", 1000, "onion", "CONTAINS FIRE!"),
                    new MenuItem("The Broken Burger", "https://i2-prod.liverpoolecho.co.uk/incoming/article16554288.ece/ALTERNATES/s1200b/0_KFC-biggest-burger-UK-1676790.jpg", 1200, "vitamins", "Basically Thanksgiving leftovers!"),
                    new MenuItem("The Bleeding Burger", "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSKkVVnVelSVQn5udkotyToGxWT3k82h2Nugg&usqp=CAU", 2000, "lactose", "All the calories you need in a day")];
*/

export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      //burgers:burgerSelection
        burgers: menu,
        fn:'',
        em:'',
        sn:'',
        hn:'',
        pmt:'',
        gr:''
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
    },
    placeOrder: function() {
      console.log([this.fn, this.em, this.sn, this.hn, this.pmt, this.gr])
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

  body{
    font-family: "Calibri", sans-serif;
    font-size: 1.2em;
  }

  section {
    margin: 0em 1.1em 0em 1.1em;
    padding: 1.1em 1.1em 1.1em;
  }

  button {
    margin: 0.6em 1.7em 0.6em 1.7em;
  }

  button:hover {
    background-color: DarkOrchid;
  }

  .allergy{
    font-weight: Bold;
  }

  .Burger {
    padding: 1.1em 1.1em 2.2em
  }

  .Title {
    height: 10em;
    overflow:hidden;
    margin-right: 1.1em;
    margin-left: 1.1em;
  }


  header > h1 {
    position: absolute;
    padding: 7em 1.1em 7em;
    margin-top: -10em;
  }

  header > img {
    opacity: 0.80;
    width: 100%;
    height: auto;
  }

  #BurgerSelect {
    color: DarkGreen;
    background-color: BurlyWood;
    border:0.1em Dashed Black;
  }

  #CustomerInfo {
    border: 0.1em Dashed CadetBlue;
    color: CadetBlue;
  }

  .wrapper {
    display: grid;
    grid-gap: 0.4em;
    grid-template-columns: 33% 33% 33%;
  }


</style>
