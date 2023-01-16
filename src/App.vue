<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>
    <input type="text" v-model="query" placeholder="Type some text" @input="changeShowList"/>
    <div id="sec">
      <ul>
        <li v-if="showList" v-for="item in searchList" @click="clickedItem(item)" >
          <img :src="item.avatar_url" alt="img">
          <span>{{ item.login }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      msg: 'Vue Type Ahead',
      query: '',
      searchList: [],
      timer: '',
      showList: true,
    }
  },
  watch:{
    query(val){
      console.log(val)
      this.query = val;
      this.showSearchListDebounce()
    }
  },
  methods:{
    changeShowList: function(){
      console.log("Change show list called")
      this.showList = true;
    },
    showSearchListDebounce(){
      if(this.query.length > 1 && this.showList){
      clearTimeout(this.timer)
      this.timer = setTimeout(()=>{
          fetch('https://api.github.com/search/users?q=' + this.query + '+in:login',{method:'GET',body:null}).then((res)=>res.json()
          ).then((result)=>{
              this.searchList = result.items;
              this.showList = true;
              console.log(this.searchList)
          })
      },1000)
      }else{
        this.searchList = [];
      }
    },
    clickedItem: function(item){
      console.log("clicke item")
      this.query = item.login;
      this.showList = false;
    },
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

h1, h2 {
  font-weight: bold;
  color: white;
  padding: 20px;
  background-image: radial-gradient( blue, #42b983);
}

input[type=text]{
  height: 3em;
  width: 20em;
  border-radius: 10px;
  color: white;
  padding: 10px;
  font-size: 1em;
  font-weight: bold;
  background-color:cadetblue;
  text-align: center;
}
ul {
  list-style-type: none;
  padding: 0;
}

li {
  /* display: inline-block; */
  margin: 0 10px;
  padding: 10px;
  width: 300px;
  background-color: #42b983;
} 
#sec{
  display:flex;
  flex-direction: column;
  align-items: center;
  overflow: auto;
  height: 500px;
}
img{
  height: 30px;
  width: 30px;
  border-radius: 50%;
}

a {
  color: #42b983;
}
</style>
