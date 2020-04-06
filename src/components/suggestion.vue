<template>
    <div class="suggestor">
        <input
            v-model="searchText"
            type="search"
            @focus="focus"
            @blur="blur"
            @input="inputChange"
            @keydown.enter.prevent="keyEnter"
            @keydown.up.prevent="keyUp"
            @keydown.down.prevent="keyDown"
            class="input"
        />
        <ul v-if="showList" name="suggestionList">
            <li 
              v-for="(item,index) in items" 
              :key="index" 
              :class="cursor === index? 'active box':'box'"
              @click="selectItem(item)"
              @mouseover="cursor = index"
            >
            <Template  :item="item"/>
            </li>
        </ul>
         </div>
</template>

<script>
import json from '../json/csvjson.json'
import Template from './itemTemplate'
export default {
    data: function(){
        return{
            searchText: '',
            showList: false,
            cursor: 0,
            items:[]
        }
    },
    components:{
        Template
    },
    props:{
        type: String,
        text: Object,
    },
    watch:{
        text(val){
            this.searchText =this.setLabel(val);
        }
    },
    methods: {
    change(text){
      if(text ===''){
            this.searchText = this.selectItem(null);
      }
      else{
      switch(this.type){
          case "subDistrict":{
            const searchdata = Array.from(json).filter((data)=>{ return new RegExp(text.toLowerCase()).test(data.subDistrict.substring(0,text.length).toLowerCase());}).slice(0,5);
            this.items =searchdata;
            break;
          }
          case "district":{
            const searchdata = Array.from(json).filter((data)=>{ return new RegExp(text.toLowerCase()).test(data.district.substring(0,text.length).toLowerCase());}).slice(0,5);
            this.items =searchdata;
            break;
          }
          case "province":{
            const searchdata = Array.from(json).filter((data)=>{ return new RegExp(text.toLowerCase()).test(data.province.substring(0,text.length).toLowerCase());}).slice(0,5);
            this.items =searchdata;
            break;
          }
          case "zipcode":{
            const searchdata = Array.from(json).filter((data)=>{ return new RegExp(text.toLowerCase()).test(data.Zipcode.toString().substring(0,text.length).toLowerCase());}).slice(0,5);
            this.items =searchdata;
            break;
          }   
      }
      }
    },
    setLabel(item){
        if(item!==null){
        switch(this.type){
            case "subDistrict":{
                return item.subDistrict;
            }
            case "district":{
                return item.district;
            }
            case "province":{
                return item.province;
            }
            case "zipcode":{
                return item.Zipcode;
            }
        }
        }
        else{
            return '';
        }
        
    },
    inputChange() {
      this.showList = this.searchText.length>=1 && this.items.length>0;
      this.cursor = 0;
      this.change(this.searchText);
    },
    selectItem(item) {
      if (item) {
        this.searchText = this.setLabel(item);
      }
      this.$emit('input', item);          
    },
    blur(){
      setTimeout(() => {
        this.showList = false;
      }, 200);
    },
    focus() {
      this.showList = this.searchText.length>=1 && this.items.length>0;
    },
    keyUp() {
      if (this.cursor > 0) {
        this.cursor -= 1;
      }
    },
    keyDown() {
      if (this.cursor < this.items.length - 1) {
        this.cursor += 1;
      }
    },
    keyEnter() {
      if (this.showList && this.items[this.cursor]) {
        this.selectItem(this.items[this.cursor]);
        this.showList = false;
      }
    },
    }
}
</script>

<style scoped>
.suggestor{
    width:16.67%;
    position: relative;
}
.input {
  width: 100%;
  display: block;
  margin: 0 auto;
  padding: 0.5rem 0.7rem;
  font-size: 1.1em;
  line-height: 1.25;
  color: #464a4c;
  outline: none;
  background-color: #fff;
  background-image: none;
  background-clip: padding-box;
  border: 1px solid #cecece;
  transition: border-color ease-in-out 0.15s, box-shadow ease-in-out 0.15s;
}
.input:focus {
  border: 1px solid #023d7b;
}
.active{
  background: #f3f6fa
}
.box{
  border: 1px solid #d5d5d5;
  cursor: pointer;
  font-size:1em;
  text-align: center;
  padding: 0.5em;
}
ul{
  position: absolute;
  overflow-y: auto; 
  list-style: none; 
  margin: 0px; 
  padding: 0px; 
  width: 100%; 
  z-index: 1
}
</style>