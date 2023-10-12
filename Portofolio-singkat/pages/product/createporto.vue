<template>
  <div class="mt-5 container">
    <h3>Create</h3>
    <div class="card">
      <div class="card-header">
        <h4>
          add Portofolio
          <nuxt-link to="../menu" class="btn btn-danger float-end"
            >Back</nuxt-link
          >
        </h4>
      </div>
      <div class="card-body">
        <div v-if="isLoading">
          <Loading :title="isLoadingTitle" />
        </div>

        <form @submit.prevent="saveProduct">
          <div class="mb-3">
            <label for="">Name</label>
            <input
              type="text"
              v-model="product.name"
              class="form-control"
              name=""
              id=""
            />
            <span class="text-danger" v-if="this.errorList.name">{{
              this.errorList.name[0]
            }}</span>
          </div>
          <div class="mb-3">
            <label for="">link</label>
            <input
              type="text"
              v-model="product.link"
              class="form-control"
              name=""
              id=""
            />
            <span class="text-danger" v-if="this.errorList.link">{{
              this.errorList.link[0]
            }}</span>
          </div>
          <div class="mb-3">
            <label for="">image</label>
            <input
              type="file"
              class="form-control"
              @change="handleFileChange"
              name=""
              id=""
            />
            <span class="text-danger" v-if="this.errorList.image">{{
              this.errorList.image[0]
            }}</span>
          </div>

          <div class="mb-3">
            <label for="">Description</label>
            <input
              type="text"
              class="form-control"
              v-model="product.description"
              name=""
              id=""
            />
            <span class="text-danger" v-if="this.errorList.description">{{
              this.errorList.description[0]
            }}</span>
          </div>
          <div class="mb-3">
            <button type="submit" class="btn btn-primary">Save</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  name: 'ProductCreate',
  data() {
    return {
      product: {
        name: '',
        image: null,
        description: '',
        link: '',
      },
      isLoading: false,
      isLoadingTitle: 'Loading',
      errorList: {},
    }
  },
  methods: {
    handleFileChange(event) {
      const selectedFile = event.target.files[0] // Get the selected file from the input

      if (selectedFile) {
        this.product.image = selectedFile // Set the selected file as the 'image' property
      }
    },
    saveProduct() {
      const formData = new FormData()
      formData.append('name', this.product.name)
      formData.append('image', this.product.image)
      formData.append('description', this.product.description)
      formData.append('link', this.product.link)

      this.isLoading = true
      this.isLoadingTitle = 'Saving'
      // alert('am here')
      // const myThis=this;
      // this.handleFileChange();
      // this.product.append('avatar', this.product.image, this.product.image.name);
      axios
        .post('http://127.0.0.1:8000/api/products', formData) // Use formData here, not FormData
        .then((res) => {
          console.log(res, 'res')
          alert(res.data.message)
          this.product.name = ''
          this.product.description = ''
          this.product.link = ''
          this.product.image = null // Clear the image field
          this.isLoading = false
          this.isLoadingTitle = 'Loading'
        })
        .catch((error) => {
          if (error.response && error.response.status === 422) {
            // Handle validation errors here
            alert(
              'Validation errors occurred: ' +
                JSON.stringify(error.response.data)
            )
          } else {
            // Handle other types of errors
            console.error('Error:', error)
          }
          this.isLoading = false
          this.isLoadingTitle = 'Loading'
        })
    },
  },
}
</script>