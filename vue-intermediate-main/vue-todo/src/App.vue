<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <!-- <TodoInput v-on:하위 컴포넌트에서 발생시킨 이벤트 이름="현재 컴포넌트의 메서드 명"></TodoInput> -->
    <TodoInput v-on:addTodoItem="addOneItem"></TodoInput>
    <!-- <TodoList v-bind:내려보낼 프롭스 속성 이름="현재 위치의 컴포넌트 데이터 속성"></TodoList> -->
    <TodoList v-bind:propsdata="todoItems" 
      v-on:removeItem="removeOneItem" 
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
    addOneItem(todoItem){
      const obj = {completed : false, item: todoItem}
      localStorage.setItem(todoItem, JSON.stringify(obj)); //JSON.stringify() : js객체를 String으로 변환해줌
      this.todoItems.push(obj);
    },
    removeOneItem(todoItem,index){
      localStorage.removeItem(todoItem.item);
			this.todoItems.splice(index, 1);
    },
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
  created() {
		if (localStorage.length > 0) {
			for (let i = 0; i < localStorage.length; i++) {
				if (localStorage.key(i) !== 'loglevel:webpack-dev-server') {
					this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
				}
			}
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
