<template>
<div id="app">
 <h1 class="subtitle" id="filtertag">Filter</h1>
<div id="buttons" class="has-text-centered">
  <form action="">
    <input v-model="company"  type="radio" id="Allt" value="Allt">Allt
    <input v-model="company"  type="radio" id="Atlantsolía" value="Atlantsolía">Atlantsolia
    <input v-model="company"  type="radio" id="CostcoIceland" value="Costco Iceland"> Costco Iceland
    <input v-model="company"  type="radio" id="Dælan" value="Dælan"> Dælan 
    <input v-model="company"  type="radio" id="N1" value="N1"> N1 <br>
    <input v-model="company"  type="radio" id="Ólis" value="Olís"> Ólis
    <input v-model="company"  type="radio" id="Orkan" value="Orkan"> Orkan
    <input v-model="company"  type="radio" id="OrkanX" value="Orkan X"> Orkan X
    <input v-model="company"  type="radio" id="Skeljungur" value="Skeljungur"> Skeljungur
  </form>
</div>
<br>
<h1 id="counter">Fjölda stöðva:{{  sortedCompany(stations).length}}</h1><br>

  <div class="box is-active is-hover" v-for="station in sortedCompany(stations)"> 
    <h1 id="name2">{{station.name}}</h1>
    <div><h1 id="bensin" class="subtitle">Bensin:{{station.bensin95}}</h1></div> 
    <strong><h1 id="name">{{station.company}}</h1></strong>
    <div><h1 id="diesel" class="subtitle">Díesel:{{station.diesel}}</h1></div> 

</div>
</div>
</template>



<script>
import axios from 'axios';
export default {
  name: 'app',
  data () {
    return {
    stations:[],
    maxbensin95:179,
    company:'Allt', 
    };
  },
  computed:{
   sortedBensin: function() {
        function compare(a, b) {
          if (a.bensin95 < b.bensin95)
            return -1;
          if (a.bensin95 > b.bensin95)
            return 1;
          return 0;
        } 
        return this.stations.sort(compare);
      },
  },
  methods:{
    sortedCompany: function(stations) {
        var that = this;
        return stations.filter(function(station) {
          if(that.company == "Allt")
            return station.company;
          if(that.company != "Allt")
            return station.company == that.company;
          })
      },
  },
  mounted(){
    var self = this;
    axios.get('http://apis.is/petrol')
      .then(function(response){
        self.stations = response.data.results
        console.log(response.data)
      })
      .catch(function(errors){
        console.log(errors)
      });
  }
}
</script>
<style>

#hi:hover{
  color:red;
}
body{
  padding: 20px;
}
.subtitle{
  color:#BBBBBB;
  font-size:15px;
}
.box{
  height:100px;
  width:50%;
  margin-left:25%;
  color:white;
}
.box:hover{
  color:red;
}
#bensin{
  float:right;
  display:inline;
}
#diesel{
  float:right;
  display:inline;
}
#name{
  float:left;
  padding-top:20px;
}
#name2{
  color:black
}
#counter{
  padding-left:25%;
  font-size:20px;
}
#buttons{
  font-size:15px;
}
#filtertag{
  padding-left:25%;
}
</style>
