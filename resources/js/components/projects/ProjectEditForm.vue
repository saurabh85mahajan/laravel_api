<template>
  <div>
    <form class="shadow-md rounded px-8 pt-6 pb-8 mb-4" @submit.prevent="handleSubmit">
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
            v-model="projectName"
            class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-purple-500"
            type="text"
            value=""
          />
        </div>
      </div>
      <div class="flex items-center">
        <div class="w-1/3"></div>
        <div class="w-2/3">
          <p v-if="errorMsg.length" class="mb-4 text-red-500 text-xs italic">{{ errorMsg }}</p>
        </div>
      </div>

      <div class="flex items-center">
        <div class="w-1/3"></div>
        <div class="w-2/3">
          <button
            class="shadow bg-purple-500 hover:bg-purple-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded"
            type="submit"
          >
            Save
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
</template>

<script>
export default {
    props: ['project'],
    data() {
        return {
            projectName: this.project.name,
            errorMsg: '',
        }
    },
    methods: {
        async handleSubmit() {
            try {
                const response = await axios.put('api/projects/' + this.project.id, {name: this.projectName});
                if( response.data.status == 'OK') {
                    this.name = '';
                    this.errorMsg = '';
                    this.$emit('project-edited');
                }
            } catch (e ) {
                if(e.response.data.error.name[0].length > 0 ) {
                    this.errorMsg = e.response.data.error.name[0];
                }
            }
        },
        cancelForm() {
            this.name = '';
            this.errorMsg = '';
            this.$emit('cancel-form');
        }
    }
}
</script>