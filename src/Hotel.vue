<template>
  <div id="app">
      <div v-for="item in data_hotel.room_category">
          <h2>{{item}}</h2>
      </div>
  </div>
</template>

<script>
import dataHotel from './hotel.json';

export default {
  data(){
    return {
        data_hotel : dataHotel
    }
  }
}
</script>
