<template>
  <div id="app">
    <transition name="hidePlayer">
      <div v-show="isShow">        
        <router-view/>         
      </div> 
    </transition>
    <transition name="showPlayer">
      <player v-show="!isShow"></player>
    </transition>
    <footer is="Footer" v-show="isShow"></footer>  
  </div>
</template>

<script>
import Footer from "./components/Footer";
import Player from "./pages/Player";
export default {
  name: "app",
  components: { Footer, Player },
  computed: {
    isShow() {
      return !this.$store.state.playerShow;
    }
  },
  methods: {
    ChangePlayerShow() {
      this.$store.dispatch("changePlayerShow");
    }
  },
  created() {
    fetch("http://api.jirengu.com/fm/getChannels.php")
      .then(response => response.json())
      .then(data => {
        this.$store.dispatch("setChannels", data.channels.reverse());
      })
      .catch(e => console.log(e));
    var date = new Date();
    var date1 = new Date("2017-12-31");
    var days = parseInt((date.getTime() - date1.getTime()) / 86400000) + 1911;
    fetch(`http://qust.me:8889/api/one/vol/${days}`)
      .then(response => response.json())
      .then(data => {
        this.$store.dispatch("setOne", data);
      })
      .catch(e => {
        console.log(e);
      });
  }
};
</script>

<style>
.showPlayer-enter-active {
  transition: all 0.1s ease-out;
}
.showPlayer-leave-active {
  transition: all 0.2 ease;
}
.showPlayer-enter,
.showPlayer-leave-to {
  transform: translateX(100px);
  opacity: 0;
}
.hidePlayer-enter-active {
  transition: all 0.1s ease-out;
}
.hidePlayer-leave-active {
  transition: all 0 ease;
}
.hidePlayer-enter,
.hidePlayer-leave-to {
  transform: translateX(-100px);
  opacity: 0;
}
#app {
  width: 414px;
  height: 736px;
  margin: 0 auto;
  position: relative;
  box-shadow: 0px 0px 6px #888787;
}
</style>
