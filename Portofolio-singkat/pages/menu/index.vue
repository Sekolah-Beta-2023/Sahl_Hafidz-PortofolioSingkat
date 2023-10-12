<template>
  <div class="container mt-5">
    <div class="card">
      <div class="card-header">
        <nuxt-link to="/" class="btn btn-danger float-end">Back</nuxt-link>
        <nuxt-link
          to="../product/createporto"
          class="btn btn-primary float-end mx-3"
          >Add Portofolio</nuxt-link
        >

        <div class="card-body">
          <div v-if="isLoading">
            <Loading title="Loading..." />
          </div>
          <div v-else>
            <!-- portofolio -->
            <h4>Portofolio Lists</h4>
            <table class="table table-striped table-bordered">
              <thead>
                <th>ID</th>
                <th>name</th>
                <th>image</th>
                <th>description</th>
                <th>link</th>
                <th>created at</th>
                <th>action</th>
              </thead>
              <tbody>
                <tr v-for="(product, index) in products" :key="index">
                  <td>{{ product.id }}</td>
                  <td>{{ product.name }}</td>
                  <td>
                    <img
                      :src="`http://127.0.0.1:8000/Storage/${product.image}`"
                      style="width: 50%"
                    />
                  </td>
                  <td>{{ product.description }}</td>
                  <td>{{ product.link }}</td>
                  <td>{{ product.created_at }}</td>
                  <td>
                    <nuxt-link
                      class="btn btn-success btn-xm mx-2"
                      :to="`product/${product.id}`"
                      >Edit</nuxt-link
                    >
                    <button
                      type="button"
                      @click="deleteProduct($event, product.id)"
                      class="btn btn-danger btn-sm mx-2"
                    >
                      Delete
                    </button>
                  </td>
                </tr>
              </tbody>
            </table>
            <!-- skill -->
            <h4>Skill Lists</h4>
            <nuxt-link to="/" class="btn btn-danger float-end">Back</nuxt-link>
            <nuxt-link
              to="../skill/createskill"
              class="btn btn-primary float-end mx-3"
              >Add Portofolio</nuxt-link
            >
            <table class="table table-striped table-bordered">
              <thead>
                <th>ID</th>
                <th>name</th>
                <th>image</th>
                <th>description</th>
                <th>created at</th>
                <th>action</th>
              </thead>
              <tbody>
                <tr v-for="(skill, index) in skills" :key="index">
                  <td>{{ skill.id }}</td>
                  <td>{{ skill.name }}</td>
                  <td>
                    <img
                      :src="`http://127.0.0.1:8000/Storage/${skill.image}`"
                      style="width: 32px"
                    />
                  </td>
                  <td>{{ skill.description }}</td>
                  <td>{{ skill.created_at }}</td>
                  <td>
                    <nuxt-link
                      class="btn btn-success btn-xm mx-2"
                      :to="`../skill/${skill.id}`"
                      >Edit</nuxt-link
                    >
                    <button
                      type="button"
                      @click="deleteSkill($event, skill.id)"
                      class="btn btn-danger btn-sm mx-2"
                    >
                      Delete
                    </button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
// import Loading from '~/components/Loading.vue';

export default {
  // components: { Loading },
  name: 'product',
  data() {
    return {
      products: {},
      skills: {},
      isLoading: true,
    }
  },
  mounted() {
    this.getProduct()
    this.getSkill()
  },
  methods: {
    getProduct() {
      this.isLoading = true
      axios.get('http://127.0.0.1:8000/api/products').then((res) => {
        // console.log(res.data.products);
        this.isLoading = false
        this.products = res.data.products
      })
    },

    getSkill() {
      this.isLoading = true
      axios.get('http://127.0.0.1:8000/api/skills').then((res) => {
        // console.log(res.data.products);
        this.isLoading = false
        this.skills = res.data.skills
      })
    },

    deleteProduct(event, productId) {
      if (confirm('are you sure?')) {
        event.target.innerText = 'Deleting'
        axios
          .delete(`http://127.0.0.1:8000/api/productdelete/${productId}`)
          .then((res) => {
            event.target.innerText = 'Delete'
            this.getProduct()
          })
      }
    },

    deleteSkill(event, skillId) {
      if (confirm('are you sure?')) {
        event.target.innerText = 'Deleting'
        axios
          .delete(`http://127.0.0.1:8000/api/skilldelete/${skillId}`)
          .then((res) => {
            event.target.innerText = 'Delete'
            this.getSkill()
          })
      }
    },
  },
}
</script>
<style lang="scss" scoped>
</style>