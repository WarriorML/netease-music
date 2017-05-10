<template>
  <div class="music">
  <div class="field is-grouped">
    <p class="control is-expanded">
      <input class="input" v-model="keyWord" @keyup.enter="search" type="text" placeholder="输入查询关键字">
    </p>
    <p class="control">
      <button class="button is-danger medium" @click="search">查询</button>
    </p>
  </div>
    <hr>
    <div class="box">
      <h5 v-show="music.name!=''?true:false">正在播放-<<{{music.name}}>></h5>
      <audio :src="music.url" controls autoplay></audio>
    </div>
    <div class="columns">
      <div class="column is-3 box">
        <div class="tile is-ancestor is-vertical">
          <div class="button tile" v-for="song in album" @click="changeSong(song)">{{song.name}}</div>
        </div>
      </div>
      <div class="column is-gapless is-9">
        <!-- <div class="tile is-ancestor is-parent"> -->
          <div class="column is-3 box album" v-for="album in this.$store.state.music.albums" @click="selAlbum(album)">
            <img :src="album.pic" class="image is-128x128" alt="">
            {{album.title}}
          </div>
        <!-- </div> -->
      </div>
    </div>
  </div>
</template>

<script>
  import ApiMusic from '../services/music_api'
  export default {
    data: function () {
      return {
        music: {name: '', url: ''},
        album: [],
        keyWord:''
      }
    },
    methods: {
      search(){
        this.$store.dispatch('getSongs',this.keyWord)
      },
      //选择歌曲改变
      changeSong(song){
        this.music = song
      },
      selAlbum(album){
        this.$store.dispatch('selectAlbum', album.a_id)
        this.album = this.$store.state.music.album.songs
      }
    },
    created(){
      this.$store.dispatch('getSongs')
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.music{
  padding: 15px;
}
audio {
    width: 100%;
    height: 60px;
  }
  .album{
    display: inline-table;
    height: 130px;
    text-align: center;
  }
  .album img{
    margin: 0 auto;
  }
</style>
