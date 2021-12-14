<template>
  <div class="w-full h-full relative mt-12">
    <nav className="top-0 h-12 fixed w-screen shadow-md bg-white z-50" ></nav>
    <div class="2xl:inline-block hidden bg-white h-screen w-64 shadow float-left"></div>
    <!-- <DriverProfile /> -->
    <CarProfile />
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
import DriverProfile from './components/DriverProfile.vue'
import CarProfile from './components/CarProfile.vue'
export default { 
  name: 'App',
  components: {
    Todo,
    Receipt,
    DriverProfile,
    CarProfile
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
