<template>
    <div class="p-4 m-4 bg-white rounded flex flex-col">
        <div>
            <h1 class="text-2xl text-gray-700">Projects</h1>
        </div>

        <div class="justify-center flex">
            <table class="table-auto justify-center">
                <thead>
                  <tr>
                    <th class="border px-4 py-2">ID</th>
                    <th class="border px-4 py-2">Name</th>
                    <th class="border px-4 py-2">Tasks</th>
                    <th class="border px-4 py-2">Actions</th>
                  </tr>
                </thead>
                <tbody>
                    <tr v-for="project in projects" :key="project.id">
                        <td>{{ project.id}}</td>
                        <td>{{ project.name}}</td>
                        <td>{{ project.tasks_count}}</td>
                        <td>Actions</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            projects: []
        }
    },
    mounted() {
        axios.get('api/projects').then( (res) => {
            console.log(res);
            this.projects = res.data.data;
        });
    }
}
</script>