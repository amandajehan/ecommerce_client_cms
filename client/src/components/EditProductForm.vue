<template>
  <div id="edit-form">
    <form @submit.prevent="editProduct" class="edit-form">
      <div class="form-group">
        <label for="product-name">Name of Product</label>
        <input
          v-model="product.name"
          type="text"
          class="form-control"
          id="product-name"
          placeholder="Enter name of product"
        />
      </div>
      <div class="form-group">
        <label for="product-image-url">Image URL</label>
        <input
          v-model="product.image_url"
          type="url"
          class="form-control"
          placeholder="Enter image url"
        />
      </div>
      <div class="form-group">
        <label for="product-price">Price of Product</label>
        <input
          v-model="product.price"
          type="number"
          class="form-control"
          id="product-price"
          placeholder="Enter price of product"
        />
      </div>
      <div class="form-group">
        <label for="product-price">Stock of Product</label>
        <input
          v-model="product.stock"
          type="number"
          class="form-control"
          id="product-stock"
          placeholder="Enter stock of product"
        />
      </div>
       <div class="form-group">
        <label for="product-name">Category of Product</label>
        <input
          v-model="product.category"
          type="text"
          class="form-control"
          id="product-category"
          placeholder="Enter category of product"
        />
      </div>
      <button type="submit" class="edit-btn btn btn-light">Edit Product</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'EditProductForm',
  data () {
    return {
      product: {
        name: '',
        image_url: '',
        price: '',
        stock: '',
        category: ''
      }
    }
  },
  methods: {
    fetchProductById () {
      const id = this.$route.params.id
      this.$store
        .dispatch('fetchProductById', id)
        .then(({ data }) => {
          this.product.name = data.name
          this.product.image_url = data.image_url
          this.product.price = data.price
          this.product.stock = data.stock
          this.product.category = data.category
        })
        .catch((err) => {
          console.log(err.response)
        })
    },
    editProduct () {
      const payload = {
        id: this.$route.params.id,
        name: this.product.name,
        image_url: this.product.image_url,
        price: this.product.price,
        stock: this.product.stock,
        category: this.product.category
      }

      this.$store
        .dispatch('editProduct', payload)
        .then(({ data }) => {
          console.log('Edit product succeed')
          this.$router.push({ name: 'Dashboard' })
          this.$swal('Success', 'Edit product succeed', 'success')
          this.$store.dispatch('fetchProducts')
        })
        .catch((err) => {
          console.log(err.response)
          this.$swal('Failed Edit Product', `${err.response.data.errors}`, 'error')
        })
    }
  },
  created () {
    this.fetchProductById()
  }
}
</script>
