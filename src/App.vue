<template>
  <div id="app" class="sm-container">
    <!-- <div>
      <musician-form @add:singer="addSinger"/>
    </div>

    <div>
      <h1>Reggae Singers</h1>
      <musician-list 
        :musicians="musicians" 
        @delete:singer="deleteSinger"
        @edit:singer="editSinger"
      />
    </div> -->

    <div>
      <h2>Add a new album</h2>
      <album-form @add:album="addAlbum"/>
    </div>

    <div>
      <small v-if="loading" class="loading">Loading data...</small>
      <small v-if="error" class="error">{{ error }}</small>

      <h2>List of reggae albums</h2>
      <album-list 
        :albums="albums" 
        :loading="loading"
        @update:album="updateAlbum"
        @delete:album="deleteAlbum"
      />
    </div>

  </div>
</template>

<script>
//Components
  //Musicians
// import MusicianList from '@/components/Musicians/MusicianList.vue';
// import MusicianForm from '@/components/Musicians/MusicianForm.vue';
  //Albums
import AlbumList from '@/components/Albums/AlbumList.vue';
import AlbumForm from '@/components/Albums/AlbumForm.vue';

export default {
  name: 'App',

  mounted(){
    this.loadAlbums();
  },

  data(){
    return{
      // musicians: [],
      albums: [],
      netError: 'Error de conexión, consulta al administrador',
      loading: false,
      error: false
    }
  },

  components: {
    // MusicianList,
    // MusicianForm,
    AlbumList,
    AlbumForm
  },

  methods: {
    // addSinger(singer){
    //   //Generating id based on array length
    //   let lastId = 0,
    //         id = 0;
    //   if(this.musicians.length > 0){
    //     lastId = this.musicians[this.musicians.length - this.musicians.length].id;
    //   }
    //   id = lastId + 1;
    //   const newSinger = { id, ...singer };

    //   this.musicians = [newSinger, ...this.musicians];
    // },

    // deleteSinger(id){
    //   this.musicians = this.musicians.filter(musician => musician.id !== id);
    // },

    // editSinger(id, updatedSinger){
    //   this.musicians = this.musicians.map(singer => 
    //     singer.id === id ? updatedSinger : singer
    //   )
    // }

    async loadAlbums(){
      this.loading = true;

      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/albums');
        const data = await response.json();
        this.albums = data;
        this.success(); //Set loading and error to false
      } catch (error){
        if(!error.ok){
          this.loading = false;
          this.error = this.netError;
        }
      }
    },

    async addAlbum(album){
      this.loading = true;

      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/albums', {
          method: 'POST',
          body: JSON.stringify(album),
          headers: {
            "Content-Type": "application/json"
          }
        });

        const data = await response.json();
        this.albums = [...this.albums, data];
        this.success(); //Set loading and error to false
      } catch(error){
        if(!error.ok){ 
          this.loading = false;
          this.error = this.netError;
        }
      }
    },

    async updateAlbum(id, updatedAlbum){
      this.loading = true;

      try{
        const response = await fetch(`https://jsonplaceholder.typicode.com/albums/${id}`, {
          method: 'PUT',
          body: JSON.stringify(updatedAlbum),
          headers: {
            "Content-Type": "application/json"
          }
        });

        const data = await response.json();
        this.albums = this.albums.map(album => album.id === id ? data : album);
        this.success();
      }catch(error){
        if(!error.ok){
          this.loading = false;
          this.error = this.netError
        }
      }
    },

    async deleteAlbum(id) {
      this.loading = true;
      try {
        await fetch(`https://jsonplaceholder.typicode.com/albums/${id}`, {
          method: "DELETE"
        });

        this.albums = this.albums.filter(album => album.id !== id);
        this.success();
      } catch (error) {
        if(!error.ok){
          this.loading = false;
          this.error = this.netError;
        }
      }
    },

    success(){
      this.loading = false;
      this.error = false;
    }

  }
}
</script>

<style>

/* Global Variables */
:root{
  --main-color: lightseagreen;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.sm-container{
  display: grid; 
  grid-template-columns: 1fr 3fr;
  gap: 20px;
  margin: 10px;
  padding: 10px;
}
tr > td > button, form button{
  padding: .3rem;
  margin: .1rem;
  border: none;
  background-color: var(--main-color);
}
tr > td > button:hover, form button:hover {
  padding: .3rem;
  margin: .1rem;
  border: none;
  background-color: var(--main-color);
  color: black;
}
.loading, .error{
  font-weight: bolder;
}
.loading{
  color: royalblue;
}
.error{
  color:crimson;
}
</style>
