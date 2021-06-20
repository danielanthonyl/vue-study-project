<template>
  <base-dialog @close="confirmError" title="Invalid Input" v-if="inputIsValid">
    <template #body>
      <p>unfortunately, at least one input vlaue is invalid.</p>
      <p>Go ahead and fill them in.</p>
    </template>
    <template #actions>
      <base-button @click="confirmError">
        Okay
      </base-button>
    </template>
  </base-dialog>

  <base-card>
    <form @submit.prevent="submitData">
      <div class="form-control">
        <label for="title">Title</label>
        <input ref="titleInput" type="text" id="title" name="title" />
      </div>

      <div class="form-control">
        <label for="title">Description</label>
        <textarea
          ref="desInput"
          name="description"
          id="description"
          rows="3"
        ></textarea>
      </div>

      <div class="form-control">
        <label for="link">Link</label>
        <input ref="linkInput" id="link" name="link" type="url" />
      </div>

      <base-button>Add Resource</base-button>
    </form>
  </base-card>
</template>

<script>
export default {
  data() {
    return {
      inputIsValid: false,
    };
  },
  inject: ['addResource'],
  methods: {
    confirmError() {
      this.inputIsValid = false;
    },
    submitData() {
      const data = {
        title: this.$refs.titleInput.value,
        description: this.$refs.desInput.value,
        link: this.$refs.linkInput.value,
        id: new Date().toISOString(),
      };

      if (
        data.title.trim() === '' ||
        data.description.trim() === '' ||
        data.link.trim() === ''
      ) {
        this.inputIsValid = true;
        return;
      }

      this.addResource(data);
    },
  },
};
</script>

<style scoped>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  padding: 0.15rem;
  border: 1px solid #ccc;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: #3a0061;
  background-color: #f7ebff;
}

.form-control {
  margin: 1rem 0;
}
</style>
