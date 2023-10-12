<template>
  <div class="mt-5 container">
    <h3>Edit Skill</h3>

    <div class="card">
      <div class="card-header">
        <h4>Edit Skill</h4>
        <nuxt-link to="../../menu" class="btn btn-danger float-end"
          >Cancel</nuxt-link
        >
      </div>
      <div class="card-body">
        <div v-if="isLoading">
          <Loading :title="isLoadingTitle" />
        </div>

        <form @submit.prevent="editSkill">
          <div class="mb-3">
            <label for="name">Name</label>
            <input
              type="text"
              v-model="skill.name"
              class="form-control"
              name="name"
              id="name"
            />
            <span class="text-danger" v-if="errorList.name">{{
              errorList.name[0]
            }}</span>
          </div>
          <div class="mb-3">
            <label for="image">Image</label>
            <input
              type="file"
              class="form-control"
              @change="handleFileChange"
              name="image"
              id="image"
            />
            <span class="text-danger" v-if="errorList.image">{{
              errorList.image[0]
            }}</span>
            <p style="color: red">mohon masukan ulang gambar🙏</p>
          </div>
          <div class="mb-3">
            <label for="description">Description</label>
            <input
              type="text"
              class="form-control"
              v-model="skill.description"
              name="description"
              id="description"
            />
            <span class="text-danger" v-if="errorList.description">{{
              errorList.description[0]
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
  name: 'EditSkill',
  data() {
    return {
      skillId: this.$route.params.id,
      skill: {
        name: '',
        image: null,
        description: '',
      },
      isLoading: false,
      isLoadingTitle: 'Loading',
      errorList: {},
    }
  },

  mounted() {
    this.getSkill(this.skillId)
  },

  methods: {
    handleFileChange(event) {
      const selectedFile = event.target.files[0]

      if (selectedFile) {
        this.skill.image = selectedFile
      }
    },

    getSkill(skillId) {
      axios
        .get(`http://127.0.0.1:8000/api/skills/${skillId}`)
        .then((res) => {
          this.skill = res.data.skill
        })
        .catch((error) => {
          console.error('Error fetching skill:', error)
        })
    },

    editSkill() {
      const formData = new FormData()
      formData.append('name', this.skill.name)
      formData.append('image', this.skill.image)
      formData.append('description', this.skill.description)

      this.isLoading = true
      this.isLoadingTitle = 'Saving'

      //   Make a POST request with the "X-HTTP-Method-Override" header set to 'PUT'
      axios
        .post(
          `http://127.0.0.1:8000/api/skillsupdate/${this.skillId}`,
          formData,
          {
            headers: {
              'X-HTTP-Method-Override': 'PUT',
            },
          }
        )
        .then((res) => {
          alert(res.data.message)
          this.$router.push('../../menu') // Redirect to the skill list after successful edit
        })
        .catch((error) => {
          if (error.response && error.response.status === 422) {
            this.errorList = error.response.data.errors
            alert(
              'Validation errors occurred: ' +
                JSON.stringify(error.response.data)
            )
          } else {
            console.error('Error editing skill:', error)
          }
          this.isLoading = false
          this.isLoadingTitle = 'Loading'
        })
    },
  },
}
</script>
    