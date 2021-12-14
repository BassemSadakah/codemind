<template>
  <Loading v-if="loading"/>
  <div v-if="!loading" class="flex lg:flex-row flex-col mt-15 md:p-20 sm:p-5 p-2  gap-10">      
        
      <div class="sm:hidden lg:flex-none profile-ldiv flex-1  bg-white shadow-sm rounded-lg border-2 border-gray-100 pt-10 pb-5 lg:self-start">
            <div class="grid text-center lg:text-left grid-cols-1 lg:grid-cols-1 sm:grid-cols-2 mx-10 text-left">
                <div>
                    <p class=" text-gray-400 mt-7" >ID</p>
                    <p class="mt-1"> {{data.id}}</p>
                </div>
                <div>
                    <p class=" text-gray-400 mt-7">Brand</p>
                    <p class="mt-1">{{data.brand}}</p>
                </div>
                <div>
                    <p class=" text-gray-400 mt-7">Model</p>
                    <p class="mt-1">{{data.model}}</p>
                </div>
                <div>
                    <p class=" text-gray-400 mt-7">Code</p>
                    <p class="mt-1">{{data.code}}</p>
                </div>
                <div>
                    <p class=" text-gray-400 mt-7">Motor</p>
                    <p class="mt-1">{{data.motor}}</p>
                </div>
                <div>
                    <p class=" text-gray-400 mt-7">Rating</p>
                    <p class="mt-1">{{data.health_rating}}</p>
                </div>
            </div>
        </div>
      <div class="flex flex-row flex-wrap content-between flex-1 gap-5">
            <div class="hidden sm:block px-20 py-10 bg-white w-full shrink-0 shadow-sm  border-2 border-gray-100  rounded-lg cursor-pointer bg-cover bg-no-repeat" :Xstyle="`box-shadow: inset 0 20px 25px -4px rgb(0 0 0 / 0.1);height:220px;filter:b1lur(1.5px); background-image:url(${mapImg})`">
            <div class="grid grid-cols-1 relative">
                <p class="text-4xl font-bold text-left">{{data.brand}} Model {{data.model}}</p>
                <p class="text-lg leading-8 text-gray-500 text-left">ID: {{data.code}}</p>
                <p class="text-lg leading-8 text-gray-500 text-left">Motor: {{data.motor}}</p>
                <!-- <p class="text-2xl font-bold text-left absolute top-52">Motor</p>
                <p class="text-lg leading-8 text-gray-500 text-left absolute top-60">123-123</p> -->
            </div>
                <img class="m-auto w-120" :src="carImg"/>
                <!-- <p class="text-gray-400">4.7/5</p> -->
                <Rating :rate="data.health_rating"/>
            </div>


            <div class=" flex-1 flex flex-col items-center justify-center p-7  bg-white   shadow-sm  border-2 border-gray-100  rounded-lg" >
                <p class="text-xl font-bold mb-5">Time since leaving </p>
                <div class="inline-block">
                    <div class="flex flex-row border-2 border-dashed rounded-md border-gray-400 p-5">
                            <div>
                                <p class="text-5xl">{{leftSince.d_s}}</p>
                                <p class="text-gray-500">Days</p>
                            </div>
                                <p class="text-5xl mx-2">:</p>
                            <div>
                                <p class="text-5xl">{{leftSince.h_s}}</p>
                                <p class="text-gray-500">hrs</p>
                            </div>
                                <p class="text-5xl mx-2">:</p>
                            <div>
                                <p class="text-5xl">{{leftSince.m_s}}</p>
                                <p class="text-gray-500">mins</p>
                            </div>
                    </div>
                </div>
            </div>
            <div class="flex-1 flex flex-col items-center justify-center p-10 bg-white  shrink-0 shadow-sm  border-2 border-gray-100  rounded-lg" >
                <p class="text-xl  mb-2 font-bold">Current Location</p>
                <p class="text-lg text-lefet text-gray-500 mb-5">{{data.location.lat}}, {{data.location.lon}}</p>
                <button class="text-left bg-transparent hover:bg-green-500 text-green-500 font-semibold hover:text-white py-2 px-4 border border-green-500 hover:border-transparent rounded">View Location</button>
            </div>
        <!-- </div> -->

        <div class="flex-1 p-10 bg-white  shrink-0 shadow-sm  border-2 border-gray-100  rounded-lg">
            <!-- <p class="text-2xl mb-9 text-center">Expiration Data</p> -->
            <div class="flex sm:flex-row flex-col items-center gap-5 justify-evenly" >
                <div class="mb-5 sm:mb-0">
                    <div class="progress-radial flex flex-col items-center justify-center" :style="`background-image: conic-gradient(from 180deg,${license_end.color}  ${(license_end.d/100)*360}deg,  #ebecf0  0deg)`">
                        <div class="overlay flex flex-col items-center justify-center">
                            <p class="text-4xl font-bold">{{license_end.d}}</p>
                            <p class="text-xl text-gray-500">Days</p>
                        </div>
                    </div>
                    <p class="text-2xl mt-5 leading-6 mt-6  ">License <br/><span class="text-gray-500 font-light text-sm">(Days Left)</span></p>
                </div>
                
    
            </div>
        </div>


        
        <div class="bg-white w-full shrink-0 shadow-sm  border-2 border-gray-100  rounded-lg cursor-pointer bg-cover bg-no-repeat" :style="`box-shadow: inset 0 20px 25px -4px rgb(0 0 0 / 0.1);height:220px;filter:b1lur(1.5px); background-image:url(${mapImg})`">
            
        </div>
        

      </div>
  </div>
</template>

<script>
import axios from 'axios'
import Rating from './Rating.vue'
import Loading from './Loading.vue'
import mapImg from '../assets/map.png'
import carImg from '../assets/car.png'
function secondsToDhms(seconds) {
    seconds = Number(seconds);
    var y = Math.floor(seconds / (3600*24*365));
    var d = Math.floor(seconds / (3600*24));
    var h = Math.floor(seconds % (3600*24) / 3600);
    var m = Math.floor(seconds % 3600 / 60);
    var s = Math.floor(seconds % 60);
    var d_s=(d.toString().length!=2)?'0'+d:d
    var h_s=(h.toString().length!=2)?'0'+h:h
    var m_s=(m.toString().length!=2)?'0'+m:m
    var s_s=(s.toString().length!=2)?'0'+s:s
    return {y,d,h,m,s,d_s,h_s,m_s,s_s};
}

export default {
    props:['user_id'],
    components:{Rating,Loading},
    setup(){
        return{
            mapImg,
            carImg
        }
    },
    data(){
        return{
            data:null,
            license_end:null,
            leftSince:null,
            loading: true
        }
    },
    mounted(){
        axios
      .get('https://api.api', {timeout: 1500})
        .then(response => {
            let data = response.data.bpi

        })
        .catch(error => {
            this.data={
                "id": "2",
                "chase": "23132132",
                "model": "2018",
                "code": "23132132",
                "motor": "321321132",
                "moving": 1639122809,
                "brand": "NPX",
                "leftSince": 1639122809,
                "location": {
                    "lat": 30.232556,
                    "lon": 32.652212
                },
                "license_end": 1645122809,
                "health_rating": 4.7
                }
            //add to beforeupdate
            this.leftSince=secondsToDhms(Date.now()/1000-this.data.leftSince)
            let license_end=secondsToDhms(this.data.license_end-Date.now()/1000)
            this.license_end=license_end.d<0?{d:'0',h:'0'}:license_end
            this.license_end.color=this.license_end.d>50?'#4BB543':this.license_end.d>25?'#EED202':'#f32013'           
            console.log(error)
        })
        .finally(() => this.loading = false)
    },
    beforeUpdate(){

    }
    
}
</script>

<style>
    @media (min-width: 1024px) {
.profile-ldiv{
        width: 21.5rem;
}
    }
.progress-radial {
  position: relative;
  width: 165px;
  height: 165px;
  border-radius: 50%;
}


.progress-radial .overlay {

  position: absolute;
  width: 150px;
  height: 150px;
  background-color: white;
  border-radius: 50%;
  text-align: center;
  line-height: 60px;
  font-size: 16px;
}


</style>