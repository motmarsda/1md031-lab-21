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
        <div class = "Burger; box a" >
            <h3>The Burning Burger</h3>
            <img src="https://www.kitchensanctuary.com/wp-content/uploads/2021/05/Double-Cheeseburger-square-FS-42.jpg" alt="The Burning Burger" title="A spicy dubble paddy burger" style="height:11em;">
            <!-- CHANGED added quotation mark -->
            <!-- information about the burger in a list -->
            <u1>
              <li> 1000 kCal </li>
              <li> Contains <al class="allergy">onion</al> </li>
              <li> CONTAINS FIRE! </li>
            </u1>
            <!-- changes end tag -->
        </div>

        <div class = "Burger; box b">
            <h3>The Broken Burger</h3>
            <img src="https://i2-prod.liverpoolecho.co.uk/incoming/article16554288.ece/ALTERNATES/s1200b/0_KFC-biggest-burger-UK-1676790.jpg" alt="The Broken Burger " title="Calorie packed turkey burger" style="height:11em;">
            <u1>
              <li> 1200 kCal </li>
              <li> Contains <al class="allergy"> vitamins </al> </li>
              <li> Contains Thanksgiving leftovers! </li>
            </u1>
            <!-- changes end tag -->
        </div>

        <div class ="Burger; box c">
            <h3>The Bleeding Burger </h3>
            <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSKkVVnVelSVQn5udkotyToGxWT3k82h2Nugg&usqp=CAU" alt="The Bleeding Burger" title="Literally dripping with cheese" style="height:11em;">
            <!-- CHANGED added quotation mark -->
            <u1>
              <li> 2000 kCal </li>
              <li> Contains <al class="allergy">lactose</al> </li>
              <li> Contains all the calories you need in a day </li>
            </u1>
            <!-- changes end tag -->
        </div>
      </div>

    </section>

    <section id="CustomerInfo">
      <h2>Customer Information </h2>
      <p>Please provide the following information</p>
      <p>
          <label for="Name">Full Name</label><br>
          <input type="text" id="Name" name="fn" required="required" placeholder="First- and Last name">
      </p>
      <p>
          <label for="Email">E-mail</label><br>
          <input type="email" id="email" name="em" required="required" placeholder="E-mail address">
      </p>
      <p>
          <label for="Street Name">Street</label><br>
          <input type="text" id="Street Name" name="sn" required="required" placeholder="Street name">
      </p>
      <p>
          <label for="House Number">House</label><br>
          <input type="text" id="House Number" name="hn" required="required" placeholder="House number">
      </p>

      <p>
          <label for="recipient">Recipient</label><br>
          <select id="recipient" name="rcp">
            <option selected = "selected;">Credit Card </option>
            <option>Debit Card</option>
            <option>Swish</option>
            <option>Invoice (Klarna)</option>
          </select>
      </p>
      <p>
          <label for="Gender">Gender</label><br>
          <input type="radio" id="female" name="gr" required="required" value="female" checked="checked">
          <label for="female">Female</label><br>
          <input type="radio" id="male" name="gr" required="required" value="Male">
          <label for="male">Male</label><br>
          <input type="radio" id="nonbin" name="gr" required="required" value="Non-binary">
          <label for="nonbin">Non-binary</label><br>
          <input type="radio" id="undisclose" name="gr" required="required" value="Undisclosed">
          <label for="undisclosed">Undisclosed</label><br>
      </p>
    </section>
  </main>
  <button type="submit">
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

const socket = io();

function MenuItem (prodName, imgURL, kCal, cont, fact) {
  this.name = prodName;
  this.imageURL = imgURL;
  this.calories = kCal;
  this.content = cont;
  this.descript = fact;
}

let burgerSelection = [new MenuItem("The Burning Burger", "https://www.kitchensanctuary.com/wp-content/uploads/2021/05/Double-Cheeseburger-square-FS-42.jpg", 1000, "onion", "CONTAINS FIRE!"),
                    new MenuItem("The Broken Burger", "https://i2-prod.liverpoolecho.co.uk/incoming/article16554288.ece/ALTERNATES/s1200b/0_KFC-biggest-burger-UK-1676790.jpg", 1200, "vitamins", "Basically Thanksgiving leftovers!"),
                    new MenuItem("The Bleeding Burger", "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSKkVVnVelSVQn5udkotyToGxWT3k82h2Nugg&usqp=CAU", 2000, "lactose", "All the calories you need in a day")];

console.log(burgerSelection[2].content);
console.log("Hitta!");

export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
        burgers: burgerSelection,
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

  .box {
    border-radius: 0.4em;
    padding: 0.4em;
  }

</style>
