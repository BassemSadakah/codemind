<template>
  <div class="w-full h-full relative">
    <!-- <Todo/> -->
    <Receipt 
    :receipt_details="{car_code:'1241-7450',user_code:'4211-7412',branch:'Main',odometer:'124'}"
    :payment_details="[{name:'Lorem',quanity:2,price:175},{name:'Lorem',quanity:3,price:200},{name:'Lorem',quanity:1,price:150}]"
    :replacements="[{name:'Lorem',quanity:2,price:175},{name:'Lorem',quanity:3,price:200},{name:'Lorem',quanity:1,price:150}]"
    :taxes="[{name:'VAT',price:250}]"
    payment_status="pending"  
    />
  </div>
</template>

<script>
import Todo from './components/Todo.vue'
import Receipt from './components/Receipt.vue'
export default { 
  name: 'App',
  components: {
    Todo,
    Receipt
  }
}
</script>

<style>
*{
    font-family: 'Roboto';
}
body{
  background: #F9FAFB;

}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 160px; */
  width: 100%;
  height: 100%;
}
</style>
