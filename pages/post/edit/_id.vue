<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>EDIT</h5>
          <hr>
          <b-form @submit="update">
            <b-form-group label="Nama Depan">
              <b-form-input type="text" v-model="post.nama_depan" :class="{ 'is-invalid': validation.nama_depan }"
                placeholder="masukkan nama depan ">
              </b-form-input>
              <div v-if="validation.nama_depan" class="mt-2">
                <b-alert show variant="danger">{{ validation.nama_depan[0] }}</b-alert>
              </div>
            </b-form-group>

            <b-form-group label="Nama Belakang">
              <b-form-input type="text" v-model="post.nama_belakang" :class="{ 'is-invalid': validation.nama_belakang }"
                placeholder="masukkan nama belakang ">
              </b-form-input>
              <div v-if="validation.nama_belakang" class="mt-2">
                <b-alert show variant="danger">{{ validation.nama_belakang[0] }}</b-alert>
              </div>
            </b-form-group>

            <b-form-group label="Email">
              <b-form-textarea id="textarea" v-model="post.email" :class="{ 'is-invalid': validation.email }"
                placeholder="masukkan email" rows="5">
              </b-form-textarea>
              <div v-if="validation.email" class="mt-2">
                <b-alert show variant="danger">{{ validation.email[0] }}</b-alert>
              </div>
            </b-form-group>
            <b-button type="submit" variant="primary">UPDATE</b-button>
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

    mounted() {

      //get data post by ID
      this.$axios.get(`/api/posts/${this.$route.params.id}`)
        .then(response => {
            this.post.nama_depan   = response.data.data.nama_depan,
            this.post.nama_belakang   = response.data.data.nama_belakang,
            this.post.email = response.data.data.email
        })
    },

    methods: {

      async update(e) {
        e.preventDefault()

        //send data ke Rest API untuk update
        await this.$axios.put(`/api/posts/${this.$route.params.id}`, {

            //data yang dikirim
            nama_depan: this.post.nama_depan,
            nama_belakang: this.post.nama_belakang,
            email: this.post.email

          })
          .then(() => {
            
            //redirect ke route "post"
            this.$router.push({
              name: 'post'
            })

          })
          .catch(error => {

            //assign error validasi  
            this.validation = error.response.data
          })
      }

    }

  }
</script>

<style>

</style>