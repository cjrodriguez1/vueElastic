<template>
  <div id="app" class="container">

 <div id="example-2">
   <input type="text" v-model="id">
  <button v-on:click="addMore">Search</button>
  
  <p>Result: {{ callResult }} </p>
</div>
<div>
  <textarea v-model="message" ></textarea>
</div>
  </div>
</template>


<script>
import axios from 'axios';

export default {
  data() {
    return {
      id:'',
      query: '',
      counter : 0,
      callResult : '',
      message: 'Orale carnalito',
      element: '',
    }
  },
  methods: {
    search() {
    },
    addMore() {
      alert("Calling Elastic");
      var url = 'http://localhost:9200/esta/' + this.id;
      axios.get(url + this.element)
    .then(response => {
      // JSON responses are automatically parsed.
      this.callResult = response.data;
      console.log(this.callResult.esta.mappings.properties);
      this.message = this.callResult.esta.mappings.properties;
    })
    .catch(e => {
      this.errors.push(e)
    })
      
    },
  }
}
</script>

<style>
body {
  background-color: #E1E7E7;
}

  .bottom {
    margin-top: 50px;
    margin-left: 200px;
  }
.card-row {
  display: flex;
  justify-content: center;
  align-items: center;  
  min-width: 780px;
  width: 100%;
  height: 500px;
}
.card {
  position: relative;
  background-color: #FFFFFF;
  height: 370px;
  width: 240px;
  margin: 10px;
  overflow: hidden;
  box-shadow: 0px 2px 4px 0px rgba(0,0,0,0.5);
}
.card-image {
  position: absolute;
  left: -9999px;
  right: -9999px;
  margin: auto;
  height: 220px;
  min-width: 100%;
}
.card-footer {
  position: absolute;
  bottom: 0;
  height: 130px;
  padding: 10px 15px;
  font-family: Helvetica;
}
.card-text {
  font-size: 14px;
  color: rgba(0, 0, 0, 0.7);
}
.card-title {
  font-family: Serif;
}
.card-author {
  font-size: 14px;
  color: #BAB096;
}
</style>