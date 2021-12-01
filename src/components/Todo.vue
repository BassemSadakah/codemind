<template>
    <div id="todo" class="overflow-y-auto bg-white max-w-lg mx-auto border shadow  rounded-lg p-5 sm:mt-32 ">
        <div class="mb-5 relative">
            <h1 class=" text-3xl font-bold">Todo List</h1>
            <!-- <img :src="refetchIcon" class="w-5 absolute right-3 top-1/2 "/> -->
        </div>
        <div class="flex flex-row mb-8">
            <input ref="newTaskInput" v-model="newTask" @focus="handleFocus(true)" @blur="handleFocus(false)" class="inline self-stretch border-r-0 shadow-sm border-2 w-full border-gray-300 rounded-l py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 " type="text" placeholder="Add new task" maxlength="45" />
            <button @click="handleAddTask"  class="select-none	bg-yellow-400 rounded-r border-gray-300 border-2 border-l-0 text-2xl	px-3 font-bold" :class="{'border-indigo-500':input_focused}">+</button>
        </div>
        <div>
            <div @click="task.finished=!task.finished" v-for="(task,index) in tasks" :key="index" class="flex justify-between my-4 bg-gray-100 py-3 rounded-md text-left px-5 hover:text-gray-500 hover:line-through cursor-pointer" :class="{'text-gray-500 line-through':task.finished}"> 
                <div  class="task truncate select-none">   {{task.value}}  </div>
                <button @click="handleDeleteTask(task,index)" class=" p-2 hover:bg-yellow-400 rounded-md"><img class="w-5 select-none" :src="deleteIcon"/></button>
                
            </div>
            
        </div>
    </div>
</template>



<script>
import axios from 'axios'
import deleteIcon from '../assets/trash.svg'
import refetchIcon from '../assets/refetch.svg'
const addTaskToDB=function(task){

    // Open the indexedDB.
    var request = indexedDB.open('todo');
    request.onerror = function(event) {
        // Handle errors.
    };
    request.onupgradeneeded = function(event) {
        console.log('upgrade')
        var db = event.target.result;
        var objectStore = db.createObjectStore("tasks", { keyPath: "id",  "autoIncrement": true });
        objectStore.createIndex("value", "value", { unique: false });
        objectStore.createIndex("finished", "finished", { unique: false });
        objectStore.createIndex("synced", "synced", { unique: false });
    };
    request.onsuccess=function(e){   
        var db=e.target.result
        var request = db.transaction(["tasks"], "readwrite")
        .objectStore("tasks")
        .add(task);
        request.onsuccess = function (e) {
            this.tasks.push({id:e.target.result,...task})
            // return {id:e.target.result,...task}
        }.bind(this);
        request.onerror = function(event) {
           //handle error
        }
    }.bind(this)
}
const getTasksFromDB=function(){
    var request = indexedDB.open('todo');
    request.onsuccess=function(e){
        var db=e.target.result
         var request = db.transaction(["tasks"], "readwrite")
        .objectStore("tasks").getAll();

        request.onerror = function(event) {
            //handle error
        };    
        request.onsuccess = function(event) {
            this.tasks=request.result
        }.bind(this);
    }.bind(this)
    
}
const deleteTaskFromDB=function(task_id){
    var request = indexedDB.open('todo');
    request.onsuccess=function(e){
        var db=e.target.result
         var request = db.transaction(["tasks"], "readwrite")
        .objectStore("tasks").delete(task_id);

        request.onerror = function(event) {
            //handle error
        };
        request.onsuccess = function(event) {
            //handle success
        }
    }
}
const refetchData=function(){
    //not compelete 
     axios.post('api://api/refetcg')
        .then(({data})=>{
            let newTasks=data.tasks
            this.tasks.push(newTask)
        })

}
export default {
    mounted() {
        getTasksFromDB.call(this)
    },
    data(){
        return{
            tasks:[],
            input_focused:false,
            deleteIcon,
            refetchIcon
        }
    },
    methods:{
        handleFocus(isFocused){
            console.log(this.tasks)
            this.input_focused=isFocused
        },
        handleAddTask(){
            let newTask={value:this.newTask,finished:false}
             if(this.newTask){
                axios.post('api://api/add-task')
                    .then(({data})=>{
                        this.tasks.push(newTask)
                    })
                    .catch((error)=>{
                        addTaskToDB.call(this,newTask)
                    });
            
                
             }
            this.newTask=""
            this.$refs.newTaskInput.value = '';
        },
        handleDeleteTask(task,index){
            this.tasks.splice(index,1)
            if(task.id){
                deleteTaskFromDB.call(this,task.id)
                console.log(task.id)
            }

        }
    }
}
</script>

<style scoped>

 @media (min-width: 640px) {
     #todo{
        max-height: calc(100vh - 150px);
        margin-top: 100px;
    }}
h1{
    font-family: 'Righteous' !important;
}
span,.task{
    font-family: 'Roboto';
    font-size: 20px;
}
</style>