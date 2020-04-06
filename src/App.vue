<template>
  <div id="app">
    <Suggestion
      v-model="item"
      :text="item"
      type="subDistrict"
      @input="cal(item)"
    />
    <Suggestion
      v-model="item2"
      :text="item2"
      type="district"
      @input="cal(item2)"
    />
    <Suggestion
      v-model="item3"
      :text="item3"
      type="province"
      @input="cal(item3)"
    />
    <Suggestion
      v-model="item4"
      :text="item4"
      type="zipcode"
      @input="cal(item4)"
    />
  </div>
</template>

<script>
import Suggestion from './components/suggestion'
export default {
  name: 'App',
  components: {
    Suggestion
  },
  data: function(){
    return{
      item:null,
      item2:null,
      item3:null,
      item4:null,
    }
  },
  methods:{
    cal(val){
      this.item = val 
      this.item2 = val;
      this.item3 = val;
      this.item4 = val;
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display:flex;
}
</style>
