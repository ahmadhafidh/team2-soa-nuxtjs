<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>TAMBAH</h5>
          <hr>
          <b-form @submit="store">
            <b-form-group label="Nama Depan">
              <b-form-input type="text" v-model="post.nama_depan" :class="{ 'is-invalid': validation.nama_depan }"
                placeholder="masukkan nama depan">
              </b-form-input>
              <div v-if="validation.nama_depan" class="mt-2">
                <b-alert show variant="danger">{{ validation.nama_depan[0] }}</b-alert>
              </div>
            </b-form-group>
                        <b-form-group label="Nama Belakang">
              <b-form-input type="text" v-model="post.nama_belakang" :class="{ 'is-invalid': validation.nama_belakang }"
                placeholder="masukkan nama belakang">
              </b-form-input>
              <div v-if="validation.nama_belakang" class="mt-2">
                <b-alert show variant="danger">{{ validation.nama_belakang[0] }}</b-alert>
              </div>
            </b-form-group>
            
            <b-form-group label="Email">
              <b-form-textarea id="textarea" v-model="post.email" :class="{ 'is-invalid': validation.email }"
                placeholder="masukkan Email" rows="5">
              </b-form-textarea>
              <div v-if="validation.email" class="mt-2">
                <b-alert show variant="danger">{{ validation.email[0] }}</b-alert>
              </div>
            </b-form-group>

            <b-button type="submit" variant="primary">SIMPAN</b-button>

          </b-form>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
  export default {

    data() {
      return {
        //state post
        post: {
          nama_depan: '',
          nama_belakang: '',
          email: ''
        },
        //state validation
        validation: []
      }
    },

    methods: {
      
      //method "store"
      async store(e) {
        e.preventDefault()

        //send data ke Rest API
        await this.$axios.post('/api/posts', {

            //data yang dikirim ke server
            nama_depan:   this.post.nama_depan,
            nama_belakang:   this.post.nama_belakang,
            email: this.post.email
            
          })
          .then(() => {
            
            //redirect ke route "post"
            this.$router.push({
              name: 'post'
            })

          })
          .catch(error => {
            //assign validation  
            this.validation = error.response.data
          })
      }
    }

  }
</script>

<style>

</style>