<template>
  <h1>Card Matching</h1>
  <div id="startArea">
    <button @click="start">Start</button>
  </div>
  <div id="cards">
    <card
      class="card"
      v-for="(icon, index) in shuflledList"
      :key="index"
      :iconName="icon"
      :keepOpen="
        guessed.indexOf(icon) != -1 || openedIndexes.indexOf(index) != -1
      "
      :index="index"
      @check="checkCard"
    />
  </div>
  <div id="leaderBoard">
    <div id="timer"><span class="LeaderSpan">Chrono : </span>{{ timer }}</div>
    <div id="attempsCount">
      <span class="LeaderSpan">Attempts : </span>{{ attempsCount }}
    </div>
  </div>
</template>

<script>
import card from "./components/card.vue";
import moment from "moment";
let intervalle;
export default {
  name: "App",
  components: {
    card,
  },
  data() {
    return {
      iconList: [
        "cubes",
        "church",
        "building-columns",
        "house-chimney",
        "frog",
        "bed",
        "car-rear",
        "eye",
        "envelope-open",
        "face-grin-tongue-squint",
        "face-sad-tear",
        "fish",
      ],
      shuflledList: [],
      guessed: [],
      openedIndexes: [],
      startTime: "",
      moment: moment,
      intervalle: intervalle,
      timer: "",
      attempsCount: 0,
    };
  },
  methods: {
    start() {
      (this.attempsCount = 0),
        (this.shuflledList = this.shuflledArray([
          ...this.shuflledArray(this.iconList),
          ...this.shuflledArray(this.iconList),
        ]));
      this.startTime = moment(new Date());
      this.intervalle = setInterval(() => {
        this.timer = moment(moment(new Date()).diff(this.startTime)).format(
          "HH:mm:ss"
        );
      }, 1000);
    },
    shuflledArray(theArray) {
      let array = theArray;
      let currentIndex = array.length,
        randomIndex;

      // While there remain elements to shuffle.
      while (currentIndex != 0) {
        // Pick a remaining element.
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;

        // And swap it with the current element.
        [array[currentIndex], array[randomIndex]] = [
          array[randomIndex],
          array[currentIndex],
        ];
      }
      return array;
    },
    checkCard({ name, index }) {
      // console.log(`name : ${name} // index : ${index}`);
      // this.openedIndex = index;
      if (this.openedIndexes.length == 0) {
        this.openedIndexes.push(index);
      } else {
        this.attempsCount += 1;

        if (name == this.shuflledList[this.openedIndexes[0]]) {
          this.guessed.push(name);
          this.openedIndexes = [];
        } else {
          this.openedIndexes.push(index);
          // this.guessed.push(name);
          setTimeout(() => (this.openedIndexes = []), 1000);
          // this.openedIndex = -1;
        }
      }
      this.checkCOunt();
    },
    checkCOunt() {
      if (this.guessed.length == this.iconList.length) {
        clearInterval(this.intervalle);
      }
    },
  },
};
</script>

<style>
h1{
  /* width: 50%;
  margin: auto; */
}
.card {
  margin: 5px 10px;
}
#leaderBoard {
  display: flex;
  flex-flow: row;
  justify-content: space-between;
}
#leaderBoard div {
  background-color: rgb(78, 42, 19);
  color: white;
  width: 40%;
  margin: 2em 5em;
  /* padding: 2em; */
  text-align: center;
  /* align-content: center; */
  height: 3em;
  padding-top: 1em;
}
.LeaderSpan {
  vertical-align: middle;
  font-family: "Times New Roman", Times, serif;
  font-weight: bold;
  /* margin-right: 2em; */
}
#startArea {
  display: flex;
}
#startArea button {
  /* align-self: center; */
  color:white;
  font-weight: bold;
  cursor: pointer;
  border-radius: 5% 5% 5%;
  width: 10em;
  height: 3em;
  margin: auto;
  background-color: rgb(41, 237, 77);
}
</style>
