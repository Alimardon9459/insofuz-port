<template>
  <div class=" container__page bg-grey-1 " >
    <div class=" container__title " >
      Bugungi Buyurtmalar
    </div>
    <div class=" container__section">
      <Table :users="store.users_info" />
    </div>
  </div>
</template>

<script setup>
import Card from 'src/components/PackerComp/Card.vue'
import Table from 'src/components/PackerComp/Table.vue';
import { useApiStore } from "src/stores"
const store = useApiStore()
store.getUsersInfo()

</script>

<style lang="scss" scoped>
.container__page{
  min-height: 80vh;
}
.container__title{
  width: 100%;
  text-align: center;
  font-size: 22px;
  padding: 50px 20px 30px 20px ;
}
.container__section{
  width: 100%;
  height: auto;
}
</style>
