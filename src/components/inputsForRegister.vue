<script setup>
import { useRouter } from "vue-router";
import { useApiStore } from "src/stores";
import { ref } from "vue";
import axios from "axios";
const store = useApiStore();
const router = useRouter();
let userName = ref("");
let phoneNumber = ref("");
let address = ref("");
let comment = ref("");
let userId = ref('')
let notChecked = ref(false);
// set user-id if not in localstorage
userId.value = localStorage.getItem('userId')
let order = async () => {

    store.setupId(userId, `userId`);
    userId.value  = localStorage.getItem('userId')

    try {
      const response = await axios.post("http://insofuzlast.pythonanywhere.com/user/",{
        id: userId.value,
          userName: userName.value,
          phoneNumber: phoneNumber.value,
          address: address.value,
          // location: store.userPosition,
          comment: comment.value,
          total: store.amount,
      })
      
      notChecked.value = false;
    } catch (error) {
      console.log(error);
    }
   
};
console.log(store.purchasedProducts);
const addProducts = async () => {
  if (userId.value) {
    try {
      for (let product of store.purchasedProducts) {
        let total = product.price * product.quantity;
        const response = await axios.post('http://insofuzlast.pythonanywhere.com/orders/',{
            orderForUser: userId.value,
            image_link: product.images[0].image_link,
            name: product.name,
            quantity: product.quantity,
            size: product.size,
            gender: product.gender,
            price: product.price,
            total_price: total,
        })
      }
    

    } catch (err) {
      console.log(err.massage);
      addProducts();
    }
    router.push({ name: "user", params: { id: userId.value } });
  } else {
    notChecked.value = true;
    
  }
};
console.log(store.purchasedProducts);
const giveOrder = async ()=>{
    await order()
    await addProducts()
}

</script>
<template>
  <div class="user-info q-my-xl q-pa-md">
    <div class="title ">Ma'lumotlaringiz</div>
    <q-input v-model="userName" type="text" name="fname" label="Ismingiz" />
    <q-input v-model="phoneNumber" type="tel" name="phone"  mask="(##) ### - ## - ##" label="Raqamingiz" fill-mask />
    <q-input v-model="address" type="text" name="address" label="Manzilingiz" />
    <q-input type="textarea" v-model="comment" name="comment" label="Kamentariy berish" />
    <div class="q-my-md">
      <!-- <q-btn
        @click="order()"
        :color="notChecked == true ? 'red' : 'primary'"
        icon="location_on"
        class="q-mr-md"
      >
        locatsiya
        <q-tooltip class="bg-accent"
          >Iltimos locatsiyani olish tugmasini bosing</q-tooltip
        >
      </q-btn> -->
      <q-btn @click="giveOrder()">
        
        Buyurtma Berish
      </q-btn>
    </div>
  </div>
</template>

<style scoped>
.user-info {
  background-color: aliceblue;
}
</style>
