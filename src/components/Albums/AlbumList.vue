<template>
  <div>

    <!-- EMPTY Albums ARRAY -->
    <p v-if="noAlbums" class="empty-array">No album has been registered</p>

    <table>
      <thead>
        <tr>
          <th>Id</th>
          <th>User Id</th>
          <th>Title<th/> 
        </tr>
      </thead>
      <tbody>
        <tr v-for="album in albums" :key="album.id">

          <td>{{ album.id }}</td>

          <td v-if="active_album === album.id">
            <input type="text" v-model="album.userId"/>
          </td>
          <td v-else>{{ album.userId }}</td>

          <td v-if="active_album === album.id">
            <input type="text" v-model="album.title"/>
          </td>
          <td v-else>{{ album.title }}</td>

          <td v-if="active_album === album.id">
            <button @click="updateAlbum(album)">Save</button>
            <button @click="cancelEdit(album)">Cancel</button>
          </td>
          <td v-else>
            <button @click="editMode(album)">Edit</button>
            <button @click="$emit('delete:album', album.id)">Delete</button>
          </td>

        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'album-list',

  props: {
    albums: Array
  },

  data(){
    return {
      active_album: null
    }
  },

  methods: {
    editMode(album){
      this.cachedAlbum = Object.assign({}, album);
      this.active_album = album.id;
    },

    cancelEdit(album){
      Object.assign(album, this.cachedAlbum);
      this.active_album = null;
    },

    updateAlbum(album){
      if(album.userId === '' || album.title === '') return;
      this.$emit('update:album', album.id, album);
      this.active_album = null;
    }
  },

  computed: {
    noAlbums(){
      return this.albums.length <= 0;
    }
  }
}
</script>

<style scoped>
tbody > tr{
  color: var(--main-color);
  font-weight: bolder;
}

.empty-array{
  font-weight: bolder;
  color: tomato;
}
</style>