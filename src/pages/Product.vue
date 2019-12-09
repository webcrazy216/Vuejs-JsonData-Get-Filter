<template>
  
  <div class="page">
  <div class="row" >
    <div class="col-md-12 text-center">
     <input type="text" placeholder="Search Form" v-model="filtervalue" @change="onChange"/>
      <select name="category_id" @change="onSelectstar($event)" class="form-control" style="display: inline-block;">
        <option value="0">--- Select All Stars ---</option>
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="4">4</option>
        <option value="5">5</option>
      </select>
      <label>Prime<input type="checkbox" v-model="checked" style="height:15px; width:15px " @change="onClickHandler()"></label>
      <input type="text" placeholder="Min price" v-model="minprice" @change="onMinpricechange">
      <input type="text"  placeholder="Max price" v-model="maxprice" @change="onMaxpricechange">
      <button class="btn btn-outline-warning btn-md" @click="sortProducts('stars',1)">Sort Desc star</button>
      <button class="btn btn-outline-warning btn-md" @click="sortProducts('stars',2)">Sort Asc star</button>
      <button class="btn btn-outline-warning btn-md" @click="sortProducts('stars',0)">Unsort</button>
    </div>
  </div>
    <div class="row" style="margin:auto;" >
      <div class="comp" v-for="(item,index) in pageOfItems" :key="index">
        <Blog :title="item.title" :url="item.image" :stars="item.stars" :price="item.price" :prime="item.prime" :subscribe="item.subscribe" :free="item.free" :video="item.video"/>
      </div>
    </div>
    <div class="card-footer pb-0 pt-3">
      <jw-pagination :items="products" @changePage="onChangePage" :pageSize="pageSize"></jw-pagination>
    </div>
  </div>
</template>

<script>
import Blog from '../components/Blog.vue'
import axios from 'axios'
export default {
  name: 'product',
  props: {
    msg: String
  },
  components: {
    Blog
  },
  data(){
    return{
      originProduct:[],
      products: [],
      displaydata:[],
      tempdata: [],
      filtervalue: '',
      pageOfItems: [],
      pageSize: 10,
      selected1: 0,
      minprice: '',
      maxprice:'',
      checked:''
    }
  },
  created(){
    window.addEventListener('resize', this.onResize)
  },
  beforeDestroy(){
    window.removeEventListener('resize', this.onResize)
  },

  mounted(){
    this.onResize();
    axios.get("data.json")
    .then((response) => {
      this.tempdata = response.data
      this.products = this.tempdata
      this.originProduct = Object.assign([],this.tempdata)
    });
  },
  methods: {
    onChange() {
      this.onFilter();
    },
    onSelectstar(event){
        var selected = event.target.value*1
        
      if(selected > 0 ){
        this.selected1 = selected;
      }
      else{
        this.selected1 = 0
      }
      this.onFilter()
      
    },
    
    onMinpricechange(){
      this.onFilter()
    },
    onMaxpricechange(){
      this.onFilter()
    },
    onClickHandler(){
      this.onFilter()
    },
    // For Responsive
    onResize(){
      var size = {
        width: window.innerWidth || document.body.clientWidth,
        height: window.innerHeight || document.body.clientHeight
      }
      if(size.width<1200)
      {
        this.pageSize=6
      }
      else if(size.width<1500){
        this.pageSize=8
      }
    },

    sortProducts(param,direction) {
      if(direction===1){
        this.products = this.products.sort((a, b) => {
          return -(Number(a[param]) - Number(b[param]))
        })
      }
      if(direction===2){
        this.products = this.products.sort((a, b) => {
          return (Number(a[param]) - Number(b[param]))
        })
      }
      if(direction===0){
        this.products = Object.assign([],this.originProduct)
      }
    },

    onChangePage(pageOfItems) {
      this.pageOfItems = pageOfItems;
    },
    onFilter () {
      this.products=[]
      var maxflag = 0;
      var minflag = 0;
      if(this.maxprice == ''){
        maxflag = 1;
      }
      if(this.minprice == ''){
        minflag = 1;
      }
      for (let ind in this.tempdata) {
        var str = this.tempdata[ind].title.toLowerCase()
        var p_star = this.tempdata[ind].stars*1
        var p_price = this.tempdata[ind].price*1
        var p_prime = this.tempdata[ind].prime
        if(this.checked == true){
          if(maxflag == 1){            
            if(minflag == 1){
              if (str.includes(this.filtervalue) && p_star >= this.selected1 && p_prime) {
                this.products.push(this.tempdata[ind])
              }
            }
            else{
              if (str.includes(this.filtervalue) && p_star >= this.selected1 && p_price >= this.minprice && p_prime) {
                this.products.push(this.tempdata[ind])
              }
            }
          }
          else {
            if(minflag == 1){
              if (str.includes(this.filtervalue) && p_star >= this.selected1 && p_price <= this.maxprice && p_prime) {
                this.products.push(this.tempdata[ind])
              }
            }
            else{
              if (str.includes(this.filtervalue) && p_star >= this.selected1 && p_price >= this.minprice && p_price <= this.maxprice && p_prime) {
                this.products.push(this.tempdata[ind])
              }
            }
          }
        }
        else{
          if(maxflag == 1){            
            if(minflag == 1){
              if (str.includes(this.filtervalue) && p_star >= this.selected1) {
                this.products.push(this.tempdata[ind])
              }
            }
            else{
              if (str.includes(this.filtervalue) && p_star >= this.selected1 && p_price >= this.minprice) {
                this.products.push(this.tempdata[ind])
              }
            }
          }
          else {
            if(minflag == 1){
              if (str.includes(this.filtervalue) && p_star >= this.selected1 && p_price <= this.maxprice) {
                this.products.push(this.tempdata[ind])
              }
            }
            else{
              if (str.includes(this.filtervalue) && p_star >= this.selected1 && p_price >= this.minprice && p_price <= this.maxprice) {
                this.products.push(this.tempdata[ind])
              }
            }
          }
        }
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
 @media (min-width: 1500px){
  .comp{
    width: 20% !important;
    display: inline-block!important;
  }
}
@media (max-width: 1500px){
  .comp{
    width: 25% !important;
    display: inline-block!important;
  }
}
@media (max-width: 1200px){
  .comp{
    width: 33.3% !important;
    display: inline-block!important;
  }
}
.form-control{
  width: 200px;
  margin-left: 20px;
}
input {
    margin-left: 20px;
    height: 40px;
}
label {
  margin-left:20px;
}
</style>
