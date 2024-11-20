<template>
  <div class="homecontainer">
    <div class="backgroundheader">
      <div class="avatarbackground">
      </div>
      <div class="textheader">
        Ho N@m
      </div>
    </div>
    <SliderShow />
    <MusicTheme/>
    <div class="backgrougncontainer">
      2021 -2022
      <div class="backgroundconten">
        <div class="backgroundimg">
          <img class="imgfamily" src="../assets/family1.jpg" alt="">
        </div>
        <div class="backgroundtitle">cai khuc nao viet ca gi do cung duoc dai dai la duoc</div>
      </div>
    </div>
    <div class="backgrougncontainer">
      2021 -2022
      <div class="backgroundconten">
        <div class="backgroundtitle">cai khuc nao viet ca gi do cung duoc dai dai la duoc</div>
        <div class="backgroundimg">
          <img class="imgfamily" src="../assets/family2.jpg" alt="">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import MusicTheme from './MusicTheme.vue';
import SliderShow from './SliderShow.vue';

export default {
  name: 'HelloWorld',
  components: {
    SliderShow,
    MusicTheme
  },
  props: {
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.backgroundheader {
  background-color: #DEAA79;
  padding: 10px;
  text-align: center;
  box-sizing: border-box;
}

.avatarbackground {
  width: 80px;
  height: 80px;
  border-radius: 50px;
  overflow: hidden;
  background-image: url("../assets/avatar.jpg");
  background-size: 80px;
  border: 2px solid #659287;
  margin: auto;
  margin-bottom: -25px;
}

.textheader {
  color: #659287;
  font-weight: 900;
  font-size: 28px;
}

.backgroundconten {
  display: flex;
  width: 100%;
  box-sizing: border-box;
}

.backgrougncontainer {
  width: 100%;
  height: 200px;
  max-width: 400px;
  font-size: 14px;
  padding: 10px;
  box-sizing: border-box;
  margin: auto;
}

.backgroundimg {
  width: 60%;
  
}
.imgfamily{
    width: 100%;
    border-radius: 5px;
  }
.backgroundtitle {
  width: 40%;
  text-align: left;
  padding-left: 5px;
  font-size: 14px;
}
</style>
