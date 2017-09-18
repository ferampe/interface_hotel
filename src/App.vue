<template>
  <div id="app">
    <div class="uk-container">
      <button 
        class="uk-button uk-button-default uk-button-small uk-button-primary" 
        v-on:click="addPriceBlock()"> 
        Add Room Category
      </button>

      <br>

      <draggable v-model="price_block" :options="{handle:'.my-handle'}" @change="reordenar">

      <div 
        class="uk-margin uk-card uk-card-default uk-card-body" 
        v-for="itemPriceBlock in price_block" v-if="itemPriceBlock.status">

          <input type="text" name="daterange" value="01/01/2015 - 01/31/2015" />


          <button 
            class="uk-close-large uk-float-right" 
            type="button" 
            uk-close 
            v-on:click="deletePriceBlock(itemPriceBlock)">
          </button>

          <span class="my-handle">:::</span> 
          <input 
            class="uk-input uk-form-blank uk-form-width-large uk-form-large" 
            type="text" 
            placeholder="Form blank" 
            v-model="itemPriceBlock.name" 
            v-on:change="generarJson"> 
            
            <br>

            <a uk-icon="icon: plus-circle" v-on:click="addRoomType(itemPriceBlock)"></a>

          <draggable v-model="itemPriceBlock.room_type" :options="{handle:'.my-handle-price'}">
            <div v-for="roomType in itemPriceBlock.room_type">

              <span class="my-handle-price">::</span>
              <input 
                class="uk-input uk-form-blank uk-form-width-medium uk-form-small" 
                type="text" 
                placeholder="Name Room Type" 
                v-model="roomType.name" 
                v-on:change="generarJson">

              <input 
                type="number" 
                v-model="roomType.price" 
                v-on:change="generarJson">
                
              <a uk-icon="icon: close" v-on:click="deleteRoomType(itemPriceBlock, roomType)"></a>
            </div>
          </draggable>

      </div>

    </draggable>

    <br>

    <button v-on:click="see_json">json</button>
    <button v-on:click="ordenar">ordenar</button>

    </div>
  </div>
</template>

<script>
import dataHotel from './hotel.json';
import _ from 'lodash';
import draggable from 'vuedraggable';

export default {
  components: {
    draggable
  },
  data(){
    return {
        price_block : dataHotel.price_block,
        sortDirection: 'asc'
    }
  },
  methods:{
    
    see_json(){
        console.log(JSON.stringify(this.price_block));
    },
    addPriceBlock(){
      this.price_block.push({
            "name" : "",
            "status": 1,
            "room_type":
                [
                    {"name":"Single", "price":0},
                    {"name":"Double", "price":0},
                ]
        });
      this.generarJson();
    },
    deletePriceBlock(itemPriceBlock){
      let del = confirm("Do you want to eliminate the price?");
      if(del == true){
        var index = this.price_block.indexOf(itemPriceBlock);
        this.price_block.splice(index, 1);
      }
      this.generarJson();
    },
    addRoomType(itemPriceBlock){
      itemPriceBlock.room_type.push({"name":"", "price":0});
      this.generarJson();
    },
    deleteRoomType(itemPriceBlock, roomType){
      let del = confirm("Do you want to eliminate the price?");
      if(del == true){
        var index = itemPriceBlock.room_type.indexOf(roomType);
        itemPriceBlock.room_type.splice(index, 1);
      }
      this.generarJson();
      
    },
    ordenar(){
      this.sortDirection = (this.sortDirection == 'desc' ? 'asc' : 'desc');
    },
    reordenar(evt){
      //console.log(evt.moved.element, evt.moved.newIndex);
      this.generarJson();
    },
    generarJson(){
      console.log(JSON.stringify(this.price_block));
    }

    
  },
  computed:{
    orderByRoomCategory(){
      return _.orderBy(this.price_block, 'sort', this.sortDirection)
    }
  }
}



$(function() {
    $('input[name="daterange"]').daterangepicker();
});

</script>


<style>

.my-handle, .my-handle-price {
  cursor: move;
  cursor: -webkit-grabbing;
}

</style>

