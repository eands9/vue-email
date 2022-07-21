<template>
  <div class="home">
    <button @click="sendMail">Send Email</button>
    <img alt="Vue logo" src="../assets/logo.png">
    <button @click="sendMailWMsg">Send Email W Msg</button>
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <Products 
      v-for="product in items"
      :key="product.id"
      :product="product"
   />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import axios from 'axios'
import items from '@/data/items.js';
import Products from '@/components/Products.vue'
export default {
  name: 'Home',
  data(){
    return{
      items: items
    }
  },
  components: {
    HelloWorld, Products
  },
  methods:{
    sendMail(){
        axios.get("https://vue-emailfa.azurewebsites.net/api/sendmail").then((response) => {
        console.log(response)
      })
    },
    sendMailWMsg(){
      var content = this.items.reduce(function(a,b){
        return a + '<tr><td>' + b.id + '</a></td><td>' + b.name + '</td></tr>'
      },'')

      var formData = {
        emailSubject: "Online Order",
        // emailBody: "This is the passed email body"
        emailBody: content
      }

      // .post("/api/sendmailwmsg",formData)
      axios
        .post("http://vue-email.azurewebsites.net/api/sendmailwmsg",formData)
        .then((response) => {console.log(response)})
    }
  }
}
</script>
