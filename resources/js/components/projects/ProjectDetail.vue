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
                <h2 class="text-2xl text-gray-600 mb-4">Tasks</h2>
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
            showForm:false
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
        }
    },
    mounted() {
        this.fetchProject();
    }
}
</script>