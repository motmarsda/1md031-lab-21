<template>

  <div class = "Burger" >
      <h3 v-bind:key="burger.name"> {{ burger.name }} </h3>
      <img v-bind:src="burger.imageURL" style="height:11em;">
      <!--information about the burger in a list -->
      <u1>
        <li v-bind:key="burger.calories"> {{ burger.calories }} kCal </li>
        <li v-bind:key="burger.content" > Contains <span class="allergy">{{ burger.content }}</span> </li>
        <li v-bind:key="burger.descript"> {{ burger.descript }} </li>
      </u1>
      <button type="button" v-on:click="subFromOrder">
          <img src="https://cdn3.vectorstock.com/i/1000x1000/13/82/left-arrow-icon-vector-21641382.jpg" style = "height:1em;">
      </button>
      <span> {{ amountOrdered }}</span>
      <button type="buttom" v-on:click="addToOrder()">
          <img src="https://cdn1.vectorstock.com/i/1000x1000/88/85/right-arrow-icon-vector-21638885.jpg" style = "height:1em;">
      </button>
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
    subFromOrder: function(){
      if(this.amountOrdered>0){
        this.amountOrdered -=1;
      }
      else{
        this.amountOrdered =0;
      }

    },
    addToOrder: function(){
      this.amountOrdered +=1;
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.allergy{
	font-weight: Bold;
}

.Burger {
	padding: 1.1em 1.1em 2.2em
}

</style>
