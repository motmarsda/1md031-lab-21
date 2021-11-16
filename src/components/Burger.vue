<template>
<!-- "<div> {{ burger.name }} {{ burger.kCal }}</div>" -->

  <div class = "Burger" >
      <h3 v-bind:key="burger.name"> {{ burger.name }} </h3>
      <img v-bind:src="burger.imageURL" style="height:11em;">
      <!--information about the burger in a list -->
      <u1>
        <li v-bind:key="burger.calories"> {{ burger.calories }} kCal </li>
        <li v-bind:key="burger.content" > Contains <span class="allergy">{{ burger.content }}</span> </li>
        <li v-bind:key="burger.descript"> {{ burger.descript }} </li>
      </u1>
  </div>
</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
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
