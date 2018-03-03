<template>
      <div class="row">
        <div class="col-md-12">
          <div class="card">
            <!-- <paper-table :title="table1.title" :sub-title="table1.subTitle" :data="products" :columns="tableColumns"> -->

            <!-- </paper-table> -->
          </div>
        </div>
        <input type="text" v-model= "product.id"> Product ID <br>
        <input type="text" v-model= "product.name"> New Product Name <br>
        <input type="text" v-model= "product.price"> New Product Price <br>
        <input type="text" v-model= "product.sellerName"> Seller Name <br>
        <button v-on:click= "NewProduct"class="btn btn-primary">New Product</button>
        <div class="alert alert-success hide">updated successfully</div>
          <button v-on:click= "EditProduct"class="btn btn-primary">Edit product</button>

          <table class="table table-bordered">

          <thead>
          <thead style= "background-color: Navy">

          <tr>
          <th class="right">Product  ID </th>
          <th class="left"> Product Name </th>
          <th class="left">Product Price </th>
          <th class="left"> Creation time </th>
          <th class="left"> Updated at </th>
          <th class="center"> Seller Name </th>
          <th class="center"> Actions </th>



          </tr>
          </thead>

          <tbody style= "background-color: Yellow">

          <tr v-for="(product, index) of products.data">
          <td class="center" v-model= "current_id=product._id"> {{ product._id }} </td>
          <td class="center"> {{ product.name }} </td>
          <td class="center"> {{ product.price }} </td>
          <td class="center"> {{ product.createdAt }} </td>
          <td class="center"> {{ product.updatedAt }} </td>
          <td class="center"> {{ product.sellerName}} </td>
          <td class="center">
          <button  type="button" class='btn btn-info btn-xs'>
          <span class="glyphicon glyphicon-eye-open"></span>
 Edit this product </button>
          <button class="btn btn-danger btn-xs" v-on:click="removeProduct">
          <span class="glyphicon glyphicon-minus"> </span> Remove this product </button></td>
          </tr>
          </tbody>
          </table>
          <p> {{product.id}} </p>
      </div>
  </template>
  <script>
import axios from 'axios'
import PaperTable from 'components/UIComponents/PaperTable.vue'
const tableColumns = ['Id', 'Name', 'Price', 'CreatedAt', 'UpdatedAt', 'SellerName']

const tableData = [{
  id: 1,
  name: 'Dakota Rice',
  price: '$36.738',
  createdat: '15/2/2018',
  updatedat: '16/2/2018',
  sellername: 'Mo'
},
{
  id: 2,
  name: 'Minerva Hooper',
  price: '$36.738',
  createdat: '15/2/2018',
  updatedat: '16/2/2018',
  sellername: 'Mo'
}]

export default {
  components: {
    PaperTable
  },
  data () {
    return {
      product: {
        id: '',
        name: '',
        price: '',
        createdAt: '',
        updatedAt: '',
        sellerName: ''

      },
      products: [],
      current_id: ''

    }
  },
  Subscriptions () {
    return {
    }
  },

  created () {
    axios.get('http://localhost:3000/api/product/getProducts').then((response) => {
      // console.log(response.data)
      this.products = response.data
      console.log(this.products.data)
    })
  },
  methods: {
    NewProduct () {
      axios.post('http://localhost:3000/api/product/createProduct', {
        name: this.name,
        price: this.price,
        sellerName: this.sellerName
      })
      .then((response) => {
        console.log(response)
        window.location.reload()
      })
      .catch((error) => {
        console.log(error)
      })
    },
    removeProduct () {
      axios.delete('http://localhost:3000/api/product/deleteProduct/' + this.current_id).then((response) => {
        console.log(response)
        window.location.reload()
      })
    },
    EditProduct () {
      axios.patch('http://localhost:3000/api/product/updateProduct/' + this.product.id, {
        name: this.product.name,
        price: this.product.price
      }).then((response) => {
        console.log(response)
        window.location.reload()
      })
    }

  }
}
  </script>
  <style>
  </style>
