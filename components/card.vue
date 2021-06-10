<template>
  <div class="each-card" draggable="true" @dragstart="dragStart($event, item)">
    <div class="each-card-top" v-if="cover" :style="{backgroundColor:chooseRandom('color')}">
      <p>
        {{chooseRandom('emoji')}} {{item.title}}
      </p>
    </div>
    <div v-bind:class="classObj" style="'margin'">
      <NuxtLink :to="`/item/${item.id}/?board=${item.board}`">
        <h4>
          {{ item.title }}
        </h4>
      </NuxtLink>
    </div>
  </div>
</template>

<script>
import randoms from "../helper/randoms";

export default {
  computed: {
    
    classObj: function() {
      return {
        "each-card-head": true,
        "each-card-head-cover": this.cover
      };
    },
    
  },
  props: ["item", "board", "cover"],
  methods: {
    dragStart: function(event, item) {
      console.log("Dragging", item.title, item.id);
      event.dataTransfer.dropEffect = "move";
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.setData("id", item.id);
      event.dataTransfer.setData("board", this.board);
    },
    chooseRandom:function(type) {
    const getRandom = (min,max)=> parseInt(Math.random() * (max - min) + min);
      if (type == "emoji") {
          console.log(randoms.icons[getRandom(0, 6)],getRandom(0,6))
        return randoms.icons[getRandom(0, 6)];
      } else return randoms.colors[getRandom(0, 6)];
    },
  }
};
</script>

<style>
.each-card-top {
  background-color: #eb5a46;
  height: 100px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.25rem;
  color: #fff;
}
.each-card {
  height: fit-content;
  min-height: 2rem;
  width: 16rem;
  background-color: #fff;
  color: #111;
  margin: 0px auto;
  margin-bottom: 0.5rem;
  display: flex;
  justify-content: left;
  align-items: center;
  border-radius: 2px;
  box-shadow: 1px 1px 2px #bbb;
  flex-direction: column;
  justify-content: center;
}
.each-card-head {
  padding: 0rem 0.5rem;
}
.each-card-head-cover {
  margin: 0.5rem;
}
.each-card a {
  font: inherit;
  color: inherit;
  text-decoration: none;
}
.each-card h4 {
  font-weight: normal;
}
.each-card:hover {
  cursor: pointer;
}
</style>
