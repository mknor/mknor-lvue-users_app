<template>
  <form @submit.prevent="handleSubmit">
    <label>User Id</label>
    <input 
      type="text" 
      v-model="album.userId"
      :class="{ 'has-error': isSubmitting && invalidAlbum }"
      @focus="clearStatus"
      @keypress="clearStatus"
      ref="album"
    />

    <label>Title</label>
    <input 
      type="text" 
      v-model="album.title"
      :class="{ 'has-error': isSubmitting && invalidTitle }"
      @focus="clearStatus"
      @keypress="clearStatus"
    />

    <!-- Error Message -->
    <p v-if="error && isSubmitting" class="error-message">
      Please fill out all the required fields
    </p>

    <!-- Success Message -->
    <p v-if="success" class="success-message">
      Album added correctly
    </p>

    <button :disabled="loading">Add album</button>
  </form>
</template>

<script>
export default {
  name: 'album-form',

  props: {
    loading: Boolean
  },

  data(){
    return {
      album: {
        userId: '',
        title: ''
      },
      isSubmitting: false,
      error: false,
      success: false
    }
  },

  methods: {
    handleSubmit(){
      this.isSubmitting = true; 
      this.clearStatus(); //Clear error and success to default values

      if(this.invalidAlbum || this.invalidTitle){ // Validate empty data
        this.error = true;
        return;
      }

      this.$emit('add:album', this.album);
      this.$refs.album.focus(); // Focus on album input after submitting to add another one faster
      this.album = { userId: '', title: '' } // Clean inputs
      this.error = false;
      this.success = true;
      this.isSubmitting = false;

    },
    clearStatus(){
      this.error = false;
      this.success = false;
    }
  },
  
  computed: {
    invalidAlbum(){
      return this.album.userId === '';
    },
    invalidTitle(){
      return this.album.title === '';
    }
  }
}
</script>

<style scoped>
[class*='-message']{
  font-weight: bolder;
  text-decoration: underline;
}
.error-message{
  color: #d33c40;
}
.success-message{
  color: #32a95d;
}
</style>