<template>
  <form @submit.prevent="handleSubmit">
    <label>Singer</label>
    <input 
      type="text" 
      v-model="singer.name"
      :class="{ 'has-error': isSubmitting && invalidSinger }"
      @focus="clearStatus"
      @keypress="clearStatus"
      ref="singer"
    />

    <label>Band</label>
    <input 
      type="text" 
      v-model="singer.band"
      :class="{ 'has-error': isSubmitting && invalidBand }"
      @focus="clearStatus"
      @keypress="clearStatus"
    />

    <!-- Error Message -->
    <p v-if="error && isSubmitting" class="error-message">
      Please fill out all the required fields
    </p>

    <!-- Success Message -->
    <p v-if="success" class="success-message">
      Singer added correctly
    </p>

    <button>Add Singer</button>
  </form>
</template>

<script>
export default {
  name: 'musician-form',

  data(){
    return {
      singer: {
        name: '',
        band: ''
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

      if(this.invalidSinger || this.invalidBand){ // Validate empty data
        this.error = true;
        return;
      }

      this.$emit('add:singer', this.singer);
      this.$refs.singer.focus(); // Focus on singer input after submitting to add another one faster
      this.singer = { name: '', band: '' } // Clean inputs
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
    invalidSinger(){
      return this.singer.name === '';
    },
    invalidBand(){
      return this.singer.band === '';
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