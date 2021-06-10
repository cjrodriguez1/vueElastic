<template>
  <div id="app" class="container">
    <div class="input-group input-group-lg bottom">
      <div class="input-group-prepend">
        <span class="input-group-text">Search</span>
      </div>
      <input type="text" 
        class="form-control col-md-6" 
        @keyup.prevent="search"
        v-model="query" />
    </div>
    
    <div  class="card-row">
    <div v-for="(value, index) in data" 
      :key="index"
      :ref="`card_${index}`"
         class="card">
      
      <img class="card-image" :src="value._source.url">
      
      <div class="card-footer">
        <h3 class="card-title">{{value._source.cast_name}}</h3>
        <p class="card-text">by 
          <span class="card-author">{{value._source.og_name}}</span>
        </p>
      </div>
    </div>
  </div>

  <div id="example-1">
  <button v-on:click="counter += 1">Add 1</button>
  <p>The button above has been clicked {{ counter }} times.</p>
</div>

 <div id="example-2">
  <button v-on:click="addMore">Call Elastic</button>
  <p>Result: {{ callResult }} </p>
</div>
  </div>
</template>


<script>
import axios from 'axios';

export default {
  data() {
    return {
      query: '',
      counter : 0,
      callResult : '',
      element: '',
    }
  },
  methods: {
    search() {
        axios.get('http://localhost:5000/search?q='+this.query)
              .then(response => {
                this.data = response.data;
          })
    },
    addMore() {
      alert("Calling Elastic");
      axios.get(`http://localhost:9200/esta/` + this.element)
    .then(response => {
      // JSON responses are automatically parsed.
      this.callResult = response.data;
    })
    .catch(e => {
      this.errors.push(e)
    })
      //this.callResult += "Orale Carnalito ";
      /*#################################################
      axios.get('http://localhost:9200/esta/', {
  //data: JSON.stringify(query),
}).then((res) => {
  console.log(res);
});
#######################################################*/
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