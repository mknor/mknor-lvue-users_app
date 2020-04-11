<template>
  <div>

    <!-- EMPTY MUSICIANS ARRAY -->
    <p v-if="noMusicians" class="empty-array">No singer has been registered</p>

    <table>
      <thead>
        <tr>
          <th>Id</th>
          <th>Singer</th>
          <th>Band</th>
          <th>Actions<th/> 
        </tr>
      </thead>
      <tbody>
        <tr v-for="musician in musicians" :key="musician.id">

          <td>{{ musician.id }}</td>

          <td v-if="active_singer === musician.id">
            <input type="text" v-model="musician.name"/>
          </td>
          <td v-else>{{ musician.name }}</td>

          <td v-if="active_singer === musician.id">
            <input type="text" v-model="musician.band"/>
          </td>
          <td v-else>{{ musician.band }}</td>

          <td v-if="active_singer === musician.id">
            <button @click="editSinger(musician)">Save</button>
            <button @click="cancelEdit(musician)">Cancel</button>
          </td>
          <td v-else>
            <button @click="editMode(musician)">Edit</button>
            <button @click="$emit('delete:singer', musician.id)">Delete</button>
          </td>

        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'musician-list',

  props: {
    musicians: Array
  },

  data(){
    return {
      active_singer: null
    }
  },

  methods: {
    editMode(singer){
      this.cachedSinger = Object.assign({}, singer);
      this.active_singer = singer.id;
    },

    cancelEdit(singer){
      Object.assign(singer, this.cachedSinger);
      this.active_singer = null;
    },

    editSinger(singer){
      if(singer.name === '' || singer.band === '') return;
      this.$emit('edit:singer', singer.id, singer);
      this.active_singer = null;
    }
  },

  computed: {
    noMusicians(){
      return this.musicians.length <= 0;
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