<template>
  <Loading v-if="loading"/>
  <div v-if="!loading" class="flex lg:flex-row flex-col mt-15 md:p-20 sm:p-5 p-2  gap-10">      
      <div class=" lg:flex-none profile-ldiv flex-1  bg-white shadow-sm rounded-lg border-2 border-gray-100 pt-10 pb-5 lg:self-start">
            <!-- <img src="https://qph.fs.quoracdn.net/main-thumb-1278318002-200-ydzfegagslcexelzgsnplcklfkienzfr.jpeg" class="object-cover h-48 w-48 rounded-full m-auto border-2 border-gray-300 "/> -->
            <img :src="data?.profile" class="object-cover h-48 w-48 rounded-full m-auto border-2 border-gray-300 "/>
            <p class="text-2xl mt-5">{{this.data.name}}</p>
            <p class="text-sm text-gray-400 font-light	block sm:hidden">4128-4125-1234</p>
            <p v-show="this.last_seen=='Active now'" class="mt-1 text-green-500 bg-green-100 px-3 py-0.5 inline rounded-md" :class="{'sm:hidden':this.last_seen=='Active now'}"> Active now</p>
            <Rating :rate='data.rating'/>
            <!-- <hr class="mx-7 mt-4 border-none h-0.5 bg-gray-200"/> -->
            <hr class="mx-7 mt-4 "  :class="{'hidden sm:block':this.last_seen=='Active now'}"/>
            <div class="grid text-center lg:text-left grid-cols-1 lg:grid-cols-1 sm:grid-cols-2 mx-10 text-left">
                <div  :class="{'hidden sm:block':this.last_seen=='Active now'}">
                    <p class=" text-gray-400 mt-7" >Last Seen</p>
                    <p class="mt-1"> {{this.last_seen}}</p>
                </div>
                <div class="hidden sm:block">
                    <p class=" text-gray-400 mt-7">Code</p>
                    <p class="mt-1">{{data.code}}</p>
                </div>
                <div class="hidden sm:block">
                    <p class=" text-gray-400 mt-7">Phone Number</p>
                    <p class="mt-1">01010154654</p>
                </div>
                <div class="hidden sm:block">
                    <p class=" text-gray-400 mt-7">Rating</p>
                    <p class="mt-1">{{data.rating}}</p>
                </div>
            </div>
        </div>
      <div class="flex flex-col flex-1 gap-8">

        <div class="flex md:flex-row flex-col gap-5 ">
            <div class="flex-1 p-7 bg-white  shrink-0 shadow-sm  border-2 border-gray-100  rounded-lg" >
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
            <div class="flex-1 p-10 bg-white  shrink-0 shadow-sm  border-2 border-gray-100  rounded-lg" >
                <p class="text-xl  mb-2 font-bold">Current Location</p>
                <p class="text-lg text-lefet text-gray-500 mb-5">{{data.location.lat}}, {{data.location.lon}}</p>
                <button class="text-left bg-transparent hover:bg-green-500 text-green-500 font-semibold hover:text-white py-2 px-4 border border-green-500 hover:border-transparent rounded">View Location</button>
            </div>
        </div>

        <div class="p-10 bg-white w-full shrink-0 shadow-sm  border-2 border-gray-100  rounded-lg">
            <p class="text-2xl mb-9 text-center">Expiration Data</p>
            <div class="flex sm:flex-row flex-col items-center gap-5 justify-evenly" >
                <div class="mb-5 sm:mb-0">
                    <div class="progress-radial flex flex-col items-center justify-center" :style="`background-image: conic-gradient(from 180deg,${license_end.color}  ${(license_end.d/100)*360}deg,  #ebecf0  0deg)`">
                        <div class="overlay flex flex-col items-center justify-center">
                            <p class="text-4xl font-bold">{{license_end.d}}</p>
                            <p class="text-xl text-gray-500">Days</p>
                        </div>
                    </div>
                    <p class="text-2xl mt-5 ">License</p>
                </div>
                
                <div class="mb-5 sm:mb-0">
                    <div class="progress-radial flex flex-col items-center justify-center" :style="`background-image: conic-gradient(from 180deg,${health_end.color}  ${(health_end.d/100)*360}deg,  #ebecf0  0deg)`">
                        <div class="overlay flex flex-col items-center justify-center">
                            <p class="text-4xl font-bold">{{health_end.d}}</p>
                            <p class="text-xl text-gray-500">Days</p>
                        </div>
                    </div>
                    <p class="text-2xl mt-5">Health Cert</p>
                </div>
                <div class="mb-5 sm:mb-0">
                    <div class="progress-radial flex flex-col items-center justify-center" :style="`background-image: conic-gradient(from 180deg,${national_end.color}  ${(national_end.d/100)*360}deg,  #ebecf0  0deg)`">
                        <div class="overlay flex flex-col items-center justify-center">
                            <p class="text-4xl font-bold">{{national_end.d}}</p>
                            <p class="text-xl text-gray-500">Days</p>
                        </div>
                    </div>
                    <p class="text-2xl mt-5">National</p>
                </div>
    
            </div>
        </div>


        
        <div class="bg-white w-full shrink-0 shadow-sm  border-2 border-gray-100  rounded-lg" style="height:220px">
        </div>

      </div>
  </div>
</template>

<script>
import axios from 'axios'
import Rating from './Rating.vue'
import Loading from './Loading.vue'
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
function since_when(previous) {
  var current = Date.parse(new Date().toUTCString());
  var msPerMinute = 60 * 1000;
  var msPerHour = msPerMinute * 60;
  var msPerDay = msPerHour * 24;
  var msPerMonth = msPerDay * 30;
  var msPerYear = msPerDay * 365;

  var elapsed = current - previous;

  if (elapsed < msPerMinute) {
    let since=Math.round(elapsed / 1000)
    if(since<30){
        return 'Active now'
    }
    return  since+ ' seconds ago';
  }else if (elapsed < msPerHour) {
    return Math.round(elapsed / msPerMinute) + ' minutes ago';
  }else if (elapsed < msPerDay) {
    return Math.round(elapsed / msPerHour) + ' hours ago';
  }else if (elapsed < msPerMonth) {
    return Math.round(elapsed / msPerDay) + ' days ago';
  }else if (elapsed < msPerYear) {
    return Math.round(elapsed / msPerMonth) + ' months ago';
  }else {
    return Math.round(elapsed / msPerYear) + ' years ago';
  }
}

export default {
    props:['user_id'],
    components:{Rating,Loading},
    setup(){

    },
    data(){
        return{
            data:null,
            health_end:null,
            license_end:null,
            national_end:null,
            last_seen:null,
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
                "profile": "https://media.istockphoto.com/photos/millennial-male-team-leader-organize-virtual-workshop-with-employees-picture-id1300972574?b=1&k=20&m=1300972574&s=170667a&w=0&h=2nBGC7tr0kWIU8zRQ3dMg-C5JLo9H2sNUuDjQ5mlYfo=",
                "code": "4512-4512",
                "name": "Zeyad Mohammed",
                "active": 1639122809,
                "leftSince": 1639122809,
                "location": {
                    "lat": 30.232556,
                    "lon": 32.652211
                },
                "license_end": 1646122809,
                "health_end": 1643122809,
                "national_end": 1641122809,
                "rating": 4.7
                }
            //add to beforeupdate
            this.leftSince=secondsToDhms(Date.now()/1000-this.data.leftSince)
            let license_end=secondsToDhms(this.data.license_end-Date.now()/1000)
            this.license_end=license_end.d<0?{d:'0',h:'0'}:license_end
            let health_end=secondsToDhms(this.data.health_end-Date.now()/1000)
            this.health_end=health_end.d<0?{d:'0',h:'0'}:health_end
            let national_end=secondsToDhms(this.data.national_end-Date.now()/1000)
            this.national_end=national_end.d<0?{d:'0',h:'0'}:national_end

            this.license_end.color=this.license_end.d>50?'#4BB543':this.license_end.d>25?'#EED202':'#f32013'
            this.health_end.color=this.health_end.d>50?'#4BB543':this.health_end.d>25?'#EED202':'#f32013'
            this.national_end.color=this.national_end.d>50?'#4BB543':this.national_end.d>25?'#EED202':'#f32013'
            // this.data.active=Date.now() //To set status Active now
            this.last_seen=since_when((this.data.active*1000))
            
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


.progress-yellow     {
  /* background-image: linear-gradient(120deg, #EED202 50%, transparent 50%, transparent), linear-gradient(90deg, #EED202 50%, #ebecf0 50%, #ebecf0); */
  background-image: conic-gradient(from 180deg,#EED202  90deg,  transparent  0deg)

}


</style>