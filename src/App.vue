<template>
  <div id="app">
    <div class="app-container">
    <button  class="addlistbtn" @click = "showTextarea()"><i class="fas fa-plus plus"></i></button>
   <textarea  v-if="showTxtarea" v-model="todo" cols="40" rows="15" autofocus ></textarea>
    <button  v-if="showTxtarea" class="savebtn" @click="addTodo()">SAVE</button>
    <div v-if="!showTxtarea" :class="'list-box'">
    <ul><li v-for=" (todo,index)  in todos" :key="index" :class="'list-style'">{{Object.values(todo).toString()}}
    
    <label class="container">
    <input type="checkbox">
    <span class="checkmark"></span>
    </label>                                                              
    </li>
    </ul>
    
    </div>
    </div>

   
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
     showTxtarea: false,
     todo: "",
     todos: []
    }
  },

  methods:{
    showTextarea(){
      this.showTxtarea = true;     
    },

    addTodo(){
       
      this.$http.post("https://to-do-list-app-821e1.firebaseio.com/to-do-list/todo.json",{
        todo:this.todo

      }).then(
        function () {
          location.reload()
        }
      )
      
     

    }

   
  },

   created(){
     var arr =[];
      this.$http.get("https://to-do-list-app-821e1.firebaseio.com/to-do-list/todo.json").then(
        function (data) {
          return data.json().then(
            
            function(data){
              
              for(let key in data){
                arr.unshift(data[key]) 
              }
              
              
          
            }
          )
        }
      )
      this.todos = arr

    }
  
}
</script>

<style scoped src='./assets/css/todo.css'>


</style>
