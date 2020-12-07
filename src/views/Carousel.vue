<template>
    <div class="carousel">
    <div class="header">
      <div>{{cards[curentCard].header.text}}</div>
    </div>
    <ul class="cards" :style="{left:-width*curentCard+'px', width: width+'px'}">
      <li :style="{width: width+'px'}" v-for="(card,index) in cards" :key="index">
        <!-- for other images use srcset -->
        <img :style="{width: width+'px', height: '100vh'}" :src="card.img" alt="">
      </li>
    </ul>
      <ul class="nav">
        <li v-for="(card,index) in cards" @click="selected(index)" :key="index" v-html="index == curentCard ? '&#9679;' : '&omicron;'"></li>
      </ul>
      <a class="prev" href="#" @click.prevent="prev()">&#60;</a>
      <a class="next" href="#" @click.prevent="next()">&#62;</a>
    </div>
</template>

<script>
import tuscany from '../assets/tuscany.jpg'
import gray from '../assets/gray.jpg'

export default {
  name: 'carousel',
  props: {
  },
  data() {
    return {       
    cards: [
      {img: tuscany, header: {text: 'lorem ipsum', buttons: [{text: 'click1'},{text: 'click2'}]}},
      {img: gray, header: {text: 'lorem ipsum2', buttons: [{text: 'click3'},{text: 'click4'}]}},
      {img: tuscany, header: {text: 'lorem ipsum3', buttons: [{text: 'click5'},{text: 'click6'}]}},
      {img: gray, header: {text: 'lorem ipsum4', buttons: [{text: 'click7'},{text: 'click8'}]}},   
    ],
    curentCard: 0,
    width: 0,
    time: 0,
  }
  },
  methods: {
    next() {
      this.curentCard++;
      if (this.curentCard >= this.cards.length)
        this.curentCard = 0;
      this.reset();
    },
    prev() {
      this.curentCard--;
      if (this.curentCard < 0)
        this.curentCard = this.cards.length - 1;
      this.reset();
    },
    selected(index) {
      this.curentCard = index;
      this.reset();
    },
    reset() {
      clearInterval(this.time);
      this.play();
    },
    handleResize() {
      this.width = window.innerWidth;
    },
    play() {
      let app = this;
      this.time = setInterval(function() {
        app.next();
      }, 2000);
    }
  },
  created: function() {
    window.addEventListener('resize', this.handleResize);
    this.handleResize();
    this.play();
  },
  destroyed() {
    window.removeEventListener('resize', this.handleResize);
  },
}
</script>

<style scoped lang="scss">
.carousel {
  margin: 0 auto;
  padding: 0;
  height: 100vh;
  position: relative;
  overflow: hidden;
  .header{
    width: 100%;
    position: absolute;
    top: 50%;
    left: 0;
    padding: 0;
    margin: 0 0 20px 0;
    text-align: center;
    z-index: 1;
  }
  ul.cards {
    height: 100%;
    margin: 0;
    padding: 0;
    display: table;
    position: absolute;
    top: 0;
    transition: left 800ms;
    li {
      list-style-type: none;
      display: table-cell;
    }
  }
  ul.nav {
    width: 100%;
    position: absolute;
    bottom: 0;
    left: 0;
    padding: 0;
    margin: 0 0 20px 0;
    text-align: center;
    z-index: 1;    
    li {
      list-style-type: none;
      display: inline;
      color: #fff;
      cursor: pointer;
      padding: 0 5px;      
      font-size: 20px;       
    }
  }  
  .prev,.next{
    text-decoration: none;
    color: #fff;
    position: absolute;  
    z-index: 1;
    font-size: 50px;
    top: 50%;
  }
  .prev{
    left: 20px;
  }
  .next{
    right: 20px;
  }
}  
</style>
