<template>
  <div id="app" class="container">

 <div id="indexSection">
  <spam style="font-weight:bold">Index:</spam> <input type="text"  v-model="index">
</div>

 <div id="idSection">
  <spam style="font-weight:bold">ID:</spam> <input type="text"  v-model="id" required> 
  <button v-on:click="addMore">Search</button>
  
  
</div>
<div id="resultsArea" style="display:none">
  <div><spam style="font-weight:bold">Date:</spam> <input type="text"  v-model="date"></div>
  <div> <spam style="font-weight:bold">Message:</spam> <input type="text"  v-model="message"></div>
  <div><spam style="font-weight:bold">Level:</spam> <input type="text"  v-model="level"></div>
  <div><spam style="font-weight:bold">Process:</spam> <input type="text"  v-model="process"></div>
  <div><spam style="font-weight:bold">Direction:</spam> <input type="text"  v-model="direction"></div>
  <div><spam style="font-weight:bold">Thread:</spam> <input type="text"  v-model="thread"></div>
  <div><spam style="font-weight:bold">Event:</spam> <input type="text"  v-model="event"></div>

  <spam style="font-weight:bold">Raw JSON:</spam> <p> {{ callResult }} </p>
 
</div>
  </div>
</template>


<script>
import axios from 'axios';

export default {
  data() {
    return {
      index: '',
      id:'',
      query: '',
      counter : 0,
      callResult : '',
      element: '',
      date: '',
      level: '',
      process: '',
      direction: '',
      thread: '',
      event: '',
      message: '',
    }
  },
  methods: {
    search() {
    },
    addMore() {
      if(this.index.length == 0){
        alert("Index Missing")
      }
      if(this.id.length == 0){
        //alert("Please Enter a Valid Id Value");
      }
      else {
            this.callResult = '';
            var url = 'http://localhost:9200/' + this.index + '/_doc/' + this.id;
            axios.get(url + this.element)
          .then(response => {
            // JSON responses are automatically parsed.
            this.callResult = response.data;
            console.log(this.callResult);
            this.date = this.callResult._source.DATE;
            this.level = this.callResult._source.LEVEL;
            this.process = this.callResult._source.PROCESS;
            this.direction = this.callResult._source.DIRECTION;
            this.thread = this.callResult._source.THREAD;
            this.event = this.callResult._source.EVENT;
            this.message = this.callResult._source.MESSAGE;
          })
          .catch(e => {
            this.errors.push(e)
          })
      }
      if(this.callResult.length == 0){
        this.callResult = "No Results Found for ID: " + this.id;
      }
      document.getElementById("resultsArea").style.display = "block";
      /*else {
        document.getElementById("resultsArea").style.display = "block";
      }*/
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

  input[type=text], select {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

button {
  width: 25%;
  background-color: #4CAF50;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

div {
  border-radius: 5px;
  background-color: #f2f2f2;
  padding: 20px;
}
</style>