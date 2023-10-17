<template>
  <div>
    <form @submit.prevent="save">
      <div class="form-group">
        <label>Name</label>
        <input type="text" v-model="product.name" class="form-control" placeholder="Enter name">
      </div>
      <div class="form-group">
        <label>Detail</label>
        <input type="text" v-model="product.detail" class="form-control" placeholder="Enter detail">
      </div>

      <button type="submit" class="btn btn-primary">Submit</button>
    </form>

    <h2>Product</h2>

    <table class="table">
      <thead class="thead-light">
        <tr>
          <th scope="col">Id</th>
          <th scope="col">Name</th>
          <th scope="col">Details</th>
          <th scope="col">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in result" :key="product.id">
          <th scope="row">{{ product.id }}</th>
          <td>{{ product.name }}</td>
          <td>{{ product.detail }}</td>
          <td>
            <button class="btn btn-primary" @click="edit(product)">Edit</button>
            <button class="btn btn-danger" @click="deleteData(product)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
/* eslint-disable */
import axios from 'axios'

export default {
  name: 'Product',
  data() {
    return {
      result: [],
      product: {
        name: '',
        detail: ''
      }
    }
  },
  created() {
    this.loadTasks()
  },
  methods: {
    loadTasks() {
      const page = 'http://127.0.0.1:8000/api/products'
      axios.get(page)
        .then(({ data }) => {
          this.result = data
        })
    },
    save() {
      if(this.product.id == ''){
        this.productCreate()
      }
      else {
        this.updateData()
      }
    },
    productCreate() {
      axios.post('http://127.0.0.1:8000/api/product', this.product)
        .then(({data}) => {
          this.product.name = '',
          this.product.detail = '',
          this.product.id = '',
          alert('Product saved')
          this.loadTasks()
        })
    },
    edit (product) {
      this.product = product
    },
    updateData() {
      var url = 'http://127.0.0.1:8000/api/products/update/'+this.product.id
      axios.put(url, this.product).then(({data}) =>{ 
          this.product.name = '',
          this.product.detail = '',
          this.product.id = '',
          alert("Updated");
          this.loadTasks();
        }
      )
    },
    deleteData(product){
      var url = 'http://127.0.0.1:8000/api/products/delete/'+ product.id
      axios.delete(url);
          alert("Deleted");
          this.loadTasks();
        
    }
  }
}
</script>
