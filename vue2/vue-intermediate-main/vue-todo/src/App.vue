<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <!-- <TodoInput v-on:하위 컴포넌트에서 발생시킨 이벤트 이름="현재 컴포넌트의 메서드 명"></TodoInput> -->
    <TodoInput></TodoInput>
    <!-- <TodoList v-bind:내려보낼 프롭스 속성 이름="현재 위치의 컴포넌트 데이터 속성"></TodoList> -->
    <TodoList v-bind:propsdata="todoItems" 
      v-on:toggleItem="toggleOneItem"></TodoList>
    <TodoFooter v-on:clearAll="clearAllItem"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoFooter from './components/TodoFooter.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'

export default {
  data(){
    return{
      todoItems:[]
    }
  },
  methods:{
    toggleOneItem(todoItem, index){
      this.todoItems[index].completed = !this.todoItems[index].completed;
			localStorage.removeItem(todoItem.item);
			localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    },
    clearAllItem(){
      localStorage.clear();
      this.todoItems = [];
    }
  },
  components:{
    //컴포넌트 태그명 : 컴포넌트 내용
    //'TodoList' : TodoList

    //향상된 객체 사용
    TodoHeader,
    TodoFooter,
    TodoInput,
    TodoList
  }
}
</script>

<style>
body{
  text-align: center;
  background-color: #f6f6f6;
}

input{
  border-style: groove;
  width: 200px;
}

button{
   border-style: groove;
}

.shadow{
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
}
</style>
