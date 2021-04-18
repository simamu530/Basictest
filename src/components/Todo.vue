<!-- Todo.vue -->
<template>
   <div>
       <h3>{{todo_title}}</h3>
       <table>
           <tr>
               <th>Index</th>
               <th>Title</th>
               <th>Important</th>
               <th>Completed</th>
           </tr>
           <tr v-for="(todo, index) in todos" v-bind:key="index">
               <td>{{index}}</td>
               <td>{{todo.title}}</td>
               <td>{{todo.important ? 'Yes' : 'No'}}</td>
               <td><input type="checkbox" v-model="todo.completed"></td>
               <td><button @click="deleteTodo(todo.id)">削除</button></td>
           </tr>
       </table>
       <hr/>
       TODO:<input type="text" v-model="title">{{title}}<br>
       IMPORTANT: <input type="checkbox" v-model="important"><br>
       <button @click="addTodo">Add New Todo</button>
       <button @click="editTodo">更新</button>
   </div>
</template>

<script>
import axios from "axios";
export default {
  props: {todo_title :String},
  data: function(){ 
      return {
          todos:[],//配列
          title: '',
          important: false
      }
  },
  created: function(){
       console.log(this.todos);
  },
  methods:{
      addTodo(){
          if(!this.title){
              alert("TODOに何も書いてません")
          }
          else{
              axios.post("https://still-headland-25411.herokuapp.com/api/todo",
              {title:this.title})
              .then((response)=> {console.log(response)
                //   var new_todo = {title: this.title, important: this.important, completed: false}//{のなかは好きな関数を入れれるvueに限る}
                //   this.todos.push(new_todo);
                console.log(this.todos);
                this.todos.push(response.data.todo);
              this.todos = this.todos.filter(todo => !todo.completed)})
              .catch(({response}) => {console.log(response)});
          }
      },
      result(){
          axios.get("https://still-headland-25411.herokuapp.com/api/todo")
       .then(response => {this.todos = response.data.data});
      },
      deleteTodo: function(task_id) {
          console.log(task_id);
          axios({
              method: "delete",
              url: "https://still-headland-25411.herokuapp.com/api/todo",
              data: {
                id: task_id
              },
            })
          .then((response)=>{
          console.log(response)
            var index = this.todos.indexOf(task_id);
            this.todos.splice(index, 1);})
      },
      editTodo() {
      let newTitle = window.prompt("以下内容で更新します。");
      if (newTitle === "") {
        alert("入力欄が空欄です。");
      }
      this.edit(newTitle);
    },
    edit(index) {
console.log(index)
 // 入力された文字が出力される
      this.todos[0].addTodo = this.title;
    },
  },
  mounted() {
      this.result()
      console.log('mounted')
  }
}
</script>

<style scoped>
</style>