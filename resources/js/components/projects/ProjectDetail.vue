<template>
    <div class="p-4 m-4 bg-white rounded flex flex-col">
        <div class="flex justify-between">
            <h1 class="text-2xl text-gray-700">Project Detail #{{ id }}</h1>
            <div>
                <button @click="showForm=true" class="bg-orange-500 rounded text-white px-3 py-2 mr-6 hover:bg-orange-700">Edit</button>
                <button @click="deleteProject" class="bg-red-500 rounded text-white px-3 py-2 mr-6 hover:bg-red-700">Delete Project</button>
            </div>

        </div>

        <div class="flex justify-center" v-show="showForm">
            <project-edit-form v-if="project.id > 0" @cancel-form="showForm = false" :project="project" @project-edited="fetchProject"></project-edit-form>
        </div>

        <div v-if="project.id > 0" class="mt-6">

            <div class="flex">
                <div class="w-1/6 p-2">
                    <div class="bg-gray-400 text-gray-700 p-2 text-left">Project Name</div>
                </div>

                <div class="w-1/2 p-2">
                    <div class="bg-gray-400 text-gray-700 p-2 text-left">{{ project.name}}</div>
                </div>
            </div>

            <div class="flex">
                <div class="w-1/6 p-2">
                    <div class="bg-gray-400 text-gray-700 p-2 text-left">Created Date</div>
                </div>

                <div class="w-1/2 p-2">
                    <div class="bg-gray-400 text-gray-700 p-2 text-left">{{ project.created_at}}</div>
                </div>
            </div>

            <div class="flex">
                <div class="w-1/6 p-2">
                    <div class="bg-gray-400 text-gray-700 p-2 text-left">Updated Date</div>
                </div>

                <div class="w-1/2 p-2">
                    <div class="bg-gray-400 text-gray-700 p-2 text-left">{{ project.updated_at}}</div>
                </div>
            </div>

            <div v-if="project.tasks.length > 0" class="my-4">
                <div class="flex justify-between mb-6">
                    <h2 class="text-2xl text-gray-600 mb-4">Tasks</h2>
                    <button @click="addTask" class="bg-blue-500 rounded text-white px-3 py-2 mr-6 hover:bg-blue-700">Add Task</button>
                </div>

                <div class="flex justify-center" v-show="showTaskForm">
                    <div>
                        <form class="shadow-md rounded px-8 pt-6 pb-8 mb-4" @submit.prevent="handleTaskSubmit">
                        <div class="flex items-center mb-6">
                            <div class="w-1/3">
                            <label
                                class="block text-gray-500 font-bold md:text-right mb-1 md:mb-0 pr-4"
                                for="inline-full-name"
                            >
                                Name
                            </label>
                            </div>
                            <div class="w-2/3">
                            <input
                                v-model="task.name"
                                class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-purple-500"
                                type="text"
                                value=""
                            />
                            </div>
                        </div>
                        <div class="flex items-center">
                            <div class="w-1/3"></div>
                            <div class="w-2/3">
                            <p v-if="taskErrMsg.length" class="mb-4 text-red-500 text-xs italic">{{ taskErrMsg }}</p>
                            </div>
                        </div>

                        <div class="flex items-center">
                            <div class="w-1/3"></div>
                            <div class="w-2/3">
                            <button
                                class="shadow bg-purple-500 hover:bg-purple-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded"
                                type="submit"
                            >
                                Add
                            </button>

                            <button
                                @click="cancelForm"
                                class="shadow bg-gray-500 hover:bg-gray-400 focus:shadow-outline focus:outline-none text-white font-bold ml-2 py-2 px-4 rounded"
                                type="button"
                            >
                                Cancel
                            </button>
                            </div>
                        </div>
                        </form>
                    </div>                    
                </div>
                <task-item v-for="task in project.tasks" :key="task.id" :task="task" @delete-task="deleteTask"></task-item>
            </div>
        </div>
    </div>
</template>

<script>

import TaskItem from './TaskItem.vue';
import ProjectEditForm from './ProjectEditForm.vue';

export default {
    props: ['id'],
    components: {
        TaskItem,
        ProjectEditForm
    },
    data() {
        return {
            project: [],
            showForm:false,
            showTaskForm: false,
            task : {
                name: '',
                project_id: this.id
            },
            taskErrMsg: ''
        }
    },
    methods: {
        deleteProject() {
            axios.delete('api/projects/' + this.id).then( (res) => {
                this.$router.push({name: 'projects'});
            });
        },
        fetchProject() {
            this.showForm = false;
            axios.get('api/projects/' + this.id).then( (res) => {
                this.project = res.data.data;
            });
        },
        deleteTask( id ) {
            axios.delete('api/tasks/' + id).then( (res) => {
                //this.project.tasks
                const index = this.project.tasks.map( i => i.id).indexOf(id);
                this.project.tasks.splice(index, 1);

            });
        },
        addTask() {
            this.showTaskForm = true;
            this.task.name = '';
            this.taskErrMsg = '';
        },
        cancelForm() {
            this.showTaskForm = false;
            this.taskErrMsg = '';
        },
        async handleTaskSubmit() {
            try {
                const response = await axios.post('api/tasks', this.task);
                if( response.data.status == 'OK') {
                    this.project.tasks.push(response.data.data);
                    this.showTaskForm = false;
                    this.taskErrMsg = '';
                }
            } catch (e ) {
                if(e.response.data.error.name[0].length > 0 ) {
                    this.taskErrMsg = e.response.data.error.name[0];
                }
            }
        }

    },
    mounted() {
        this.fetchProject();
    }
}
</script>