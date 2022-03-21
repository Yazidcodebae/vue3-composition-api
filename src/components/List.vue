<template>
   <div class="row mt-5">
      <div class="col-xxl-4">
        <ul class="list-group text-start" v-for="(todo,index) in todos" :key="index">
        <li class="list-group-item">
            <div class="row gx-2 align-items-center">
                <div class="col">
                    <span v-if="todo.isDone == true">
                        <del>{{todo.activity}}</del>
                    </span>
                     <span v-else>
                        {{todo.activity}}
                    </span>
                </div>
                <div class="col text-end">
                    <button class="btn btn-danger mx-3" @click="deleteTodo(index)">X</button>
                    <div v-if="todo.isDone == true">
                        <button class="btn btn-info text-white" @click="undoneTodo(index)">Undone</button>
                    </div>
                    <div v-if="todo.isDone == false">
                        <button class="btn btn-info text-white" @click="doneTodo(index)">Done</button>
                    </div>
                </div>
            </div>
        </li>
      </ul>
      <br>
      </div>
    </div>
</template>

<script>
export default {
     props: {
        todos:{
            type: Array,
            default:[]
        }
    },
    setup(props,{emit}) {
        const deleteTodo = index => {
            emit('deleteTodo',index)
        }
        const doneTodo = index => {
            emit('doneTodo',index)
        }
        const undoneTodo = index => {
            emit('undoneTodo',index)
        }

        return {deleteTodo,doneTodo,undoneTodo}
    },
   
}
</script>

<style>

</style>