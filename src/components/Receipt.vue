<template>
    <div id="receipt" class="overflow-y-auto bg-white max-w-lg mx-auto border shadow  sm:rounded-lg p-5 sm:mt-32">
        <img class="w-20 m-auto" :src="Logo"/>
        <p class="bold mt-2 font-bold">Receipt #1546767</p>
        <div class="mt-2">
        <span class="rounded-2xl px-7 py-1 text-white" :class="{'bg-green-500':payment_status=='paid','bg-blue-500':payment_status=='pending','bg-red-500':payment_status=='refused'}">{{payment_status}}</span>
        </div>
        <hr class="my-5 border-none h-0.5 bg-gray-200"/>
        <p class="font-bold mt-2 text-xl ">Receipt Details</p>
        <div class="mt-2 mx-8 flex justify-between" >
            <div>
                <img :src="car" class="w-5 inline mr-2" />
                <span class="text-gray-500">Car Code</span>
            </div>
            <span class="">{{receipt_details.car_code}}</span>
        </div>
        <div class="mt-2 mx-8 flex justify-between">
            <div>
                <img :src="user2" class="w-5 inline mr-2" />
                <span class="text-gray-500">User Code</span>
            </div>
            <span class="">{{receipt_details.user_code}}</span>
        </div>
        <div class="mt-2 mx-8 flex justify-between">
            <div>
                <img :src="branch1" class="w-5 inline mr-2" />
                <span class="text-gray-500">Branch</span>
            </div>
            <span class="">{{receipt_details.branch}}</span>
        </div>
        <div class="mt-2 mx-8 flex justify-between">
            <div>
                <img :src="counter1" class="w-5 inline mr-2" />
                <span class="text-gray-500">Odometer</span>
            </div>
            <span class="">{{receipt_details.odometer}}</span>
        </div>
        <hr class="mt-5 mb-3 border-none h-0.5 bg-gray-200"/>
        <p class="font-bold  text-lg mb-5">Payment Details</p>
        <!-- <p class="text-left text-xl">Taxes</p> -->
        <!-- <div class="shadow rounded-md p-5 "> -->
        <table v-if="payment_details" class="w-full border-collapse ">
            <thead>
                <tr>
                    <th class="py-1 border-b-2 border-gray-200 text-gray-600">#</th>
                    <th class="border-b-2 border-gray-200 text-gray-600">Name</th>
                    <th class="border-b-2 border-gray-200 text-gray-600">Quantity</th>
                    <th class="border-b-2 border-gray-200 text-gray-600">Price</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(payment,index) in payment_details" :key="index">
                    <td class="py-1 border-b border-gray-200  text-gray-500">{{index+1}}</td>
                    <td class="border-b border-gray-200 text-gray-500">{{payment.name}}</td>
                    <td class="border-b border-gray-200 text-gray-500">{{payment.quanity}}</td>
                    <td class="border-b border-gray-200 text-gray-500">{{payment.price}}</td>
                </tr>
                <tr>
                    <td class="py-1 border-b border-gray-200  text-gray-500">Total</td>
                    <td class="border-b border-gray-200 text-gray-500">--</td>
                    <td class="border-b border-gray-200 text-gray-500">--</td>
                    <td class="border-b border-gray-200 text-gray-500">{{payment_details.reduce((a, {price}) => a + price, 0)}}</td>
                </tr>
            </tbody>
        </table>
        <!-- </div> -->
    
        <!-- <hr class="mt-5 mb-3 border-none h-0.5 bg-gray-200"/> -->
        <p v-if="replacements" class="font-bold  text-lg mb-5 mt-7">Replacements</p>
         <table v-if="replacements" class="w-full border-collapse ">
            <thead>
                <tr>
                    <th class="py-1 border-b-2 border-gray-200 text-gray-600">#</th>
                    <th class="border-b-2 border-gray-200 text-gray-600">Name</th>
                    <th class="border-b-2 border-gray-200 text-gray-600">Quantity</th>
                    <th class="border-b-2 border-gray-200 text-gray-600">Price</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(payment,index) in replacements" :key="index">
                    <td class="py-1 border-b border-gray-200  text-gray-500">{{index+1}}</td>
                    <td class="border-b border-gray-200 text-gray-500">{{payment.name}}</td>
                    <td class="border-b border-gray-200 text-gray-500">{{payment.quanity}}</td>
                    <td class="border-b border-gray-200 text-gray-500">{{payment.price}}</td>
                </tr>

                <tr>
                    <td class="py-1 border-b border-gray-200  text-gray-500">Total</td>
                    <td class="border-b border-gray-200 text-gray-500">--</td>
                    <td class="border-b border-gray-200 text-gray-500">--</td>
                    <td class="border-b border-gray-200 text-gray-500">{{replacements.reduce((a, {price}) => a + price, 0)}}</td>
                </tr>
            </tbody>
        </table>

        <!-- <hr class="mt-5 mb-3 border-none h-0.5 bg-gray-200"/> -->
        <p v-if="taxes" class="font-bold  text-xl mb-2 mt-7">Taxes</p>
         <table v-if="taxes" class="w-full border-collapse ">
            <thead>
                <tr>
                    <th class="py-1 border-b-2 border-gray-200 text-gray-600">#</th>
                    <th class="border-b-2 border-gray-200 text-gray-600">Name</th>
                    <th class="border-b-2 border-gray-200 text-gray-600">Price</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(payment,index) in taxes" :key="index">
                    <td class="py-1 border-b border-gray-200  text-gray-500">{{index+1}}</td>
                    <td class="border-b border-gray-200 text-gray-500">{{payment.name}}</td>
                    <td class="border-b border-gray-200 text-gray-500">{{payment.price}}</td>
                </tr>

                <tr>
                    <td class="py-1 border-b border-gray-200  text-gray-500">Total</td>
                    <td class="border-b border-gray-200 text-gray-500">--</td>
                    <td class="border-b border-gray-200 text-gray-500">{{taxes.reduce((a, {price}) => a + price, 0)}}</td>
                </tr>
            </tbody>
        </table>

        <div class="mx-12 mt-5 flex justify-between" >
            <span class="text-xl font-bold">Total</span>
            <span class="text-xl text-greeen-500 font-bold">500$</span>
        </div>
        <button v-if="payment_status!='paid'" class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded w-full mt-5 h-12 text-xl">Pay Now</button>
    </div>
</template>

<script>
import Logo from '../assets/logo.png'
import user1 from '../assets/user1.svg'
import user2 from '../assets/user2.svg'
import branch1 from '../assets/branch1.svg'
import branch2 from '../assets/branch2.svg'
import car from '../assets/car.svg'
import counter1 from '../assets/counter1.svg'
import counter2 from '../assets/counter2.svg'
export default {
    props:["receipt_details","payment_status","payment_details","replacements","taxes"],
    setup(){
        return{
            Logo,user1,user2,car,counter2,counter1,branch1,branch2,
        }
    },
    data(){
        return{
            // status:this.payment_status=this.payment_status.charAt(0).toUpperCase()+this.payment_status.slice(1)
        }
    }
}
</script>

<style>
@media (min-width: 640px) {
     #receipt{
        max-height: calc(100vh - 150px);
        margin-top: 100px;
    }}
</style>