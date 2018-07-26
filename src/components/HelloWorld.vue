<template>
<div class="todolist">
  <h1>todoList</h1>
  <input type="text" placeholder="what do you want?" v-model="title" @keyup.enter="addTodo">
  <button @click="addTodo">添加</button>
  <ul>
    <li v-for="item in arr">
      <input type="checkbox" v-model="item.isDone" @change="changeStatus(item.isDone,item._id)">
      <span :class="{done:item.isDone}" >{{item.title}}</span>
      <button @click="removeSelf(item._id)">删除</button>
    </li>
  </ul>
</div>
</template>

<script>
  import axios from 'axios'
export default {
  name: 'HelloWorld',
  data () {
    return {
      arr:[],
      title:""
    }
  },
  methods:{
    getData(){
      axios.get("/api/todo").then(res =>{
        console.log(res);
        this.arr = res.data.data
      })
    },
    changeStatus(value,id){
      let isDone = value?1:0;
      axios.patch(`/api/todo/${id}`,{isDone}).then(res=>{
        console.log(res);
      })
    },
    removeSelf(id){
      axios.delete(`/api/todo/${id}`).then(res =>{
        if(res.data.code == 200){
          alert("删除成功！");
          this.getData()
        }else{
          alert("删除失败！")

        }
      })
    },
    addTodo(){
      let title = this.title
      axios.post('/api/todo/',{title}).then(res =>{
        if(res.data.code == 200){
          alert("添加成功！！");
          this.getData()
        }
      })
    }
  },
  created(){
    this.getData()
  }
}
</script>


<style scoped>
  .done{
    text-decoration: line-through;
  }
</style>
