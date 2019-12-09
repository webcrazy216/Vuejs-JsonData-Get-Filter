<template>
  <div class="blog" style="text-align: left; left:20px">
    <hr >
    <img :src="url" />
    <br /><br /><br />
    <p style="font-weight: 700; font-size:15px">{{ title }}</p>
    <star-rating :increment="0.1" :fixed-points="1" :rating="stars" :star-size="20" read-only style="margin-bottom: 0; margin-top:0"/>
    <p style="font-weight: 600"><sup>$</sup>{{price}}</p>
    <p style="font-size:13px"> {{subscribe}} </p>
    <p v-if="prime" style="font-size: 13px">
      <a class="prm" style="font-size:17px; letter-spacing:-0.02em; font-weight:900;color:#2c97c4">prime</a>
      {{free}}
    </p>
    <p v-if="prime" style="font-size:13px">
      <a> Get it </a>
      <a style="font-weight:900">Tomorrow, Dec 8</a>
    </p>
    <p v-if="video">
      <button>Watch Now</button>
    </p>
</div>
</template>

<script>
import StarRating from 'vue-star-rating'
export default {
  name: 'blog',
  props: {
    msg: String,
    title: String,
    url: String,
    stars: Number,
    price: Number,
    prime: Number,
    subscribe: String,
    free:String,
    video: Number
  },
  components:{
    StarRating
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
hr{
  border: 2px solid;
}
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
.blog {
  width: 300px;
  margin: auto;
}
p{
  margin-bottom: 0;
}
.prm{
    color: #0044ff;
    font-size: 20px;
    font-weight: 600;
}
</style>
