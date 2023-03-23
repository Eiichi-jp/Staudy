<template>
    <div class="container">
        <table class="table table-hover">
            <thead class="thead-light">
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Title</th>
                    <th scope="col">Content</th>
                    <th scope="col">Person In Charge</th>
                    <th scope="col">Show</th>
                    <th scope="col">Edit</th>
                    <th scope="col">Delete</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(task, index) in tasks" :key="index">
                    <th scope="row">{{ task.id }}</th>
                    <td>{{ task.title }}</td>
                    <td>{{ task.content }}</td>
                    <td>{{ task.person_in_charge }}</td>
                    <td>
                        <router-link v-bind:to="{ name: 'task.show', params: { taskId: task.id } }">
                            <button class="btn btn-primary">Show</button>
                        </router-link>
                    </td>
                    <td>
                        <router-link v-bind:to="{ name: 'task.edit', params: { taskId: task.id } }">
                            <button class="btn btn-success">Edit</button>
                        </router-link>
                    </td>
                    <td>
                        <button class="btn btn-danger" v-on:click="deleteTask(task.id)">Delete</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
     
<script>
 //import { RouterLink } from 'vue-router';

import Axios from 'axios';

//export default { components: { RouterLink } }
export default {
    data: function () {
        return {
            tasks: []//①空の配列を用意
        }
    },
    methods: {
        getTasks() {
            Axios.get('/api/tasks').then((res) => {
                this.tasks = res.data;
                //②getTasks関数を用いて/api/tasksのapiにアクセスし返された値(res)を①で作成したtasks配列に代入
            });
        },
        deleteTask(id){
            Axios.delete('/api/tasks/' + id).then((res) => {
                this.getTasks();
            });
        }
    },
    mounted() {
        this.getTasks();//③マウントすることでコンポーネントが呼び出された際にgetTasks()が毎回実行される
    }
}
</script>