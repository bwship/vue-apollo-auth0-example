<template>
  <div class="create">
    <input v-model="description" placeholder="Description">
    <input v-model="imageUrl" placeholder="Image url">

    <button @click="create">Create Post</button>
  </div>
</template>

<script>
import gql from 'graphql-tag'

const createPost = gql`
  mutation createPost($description: String!, $imageUrl: String!) {
    createPost(description: $description, imageUrl: $imageUrl) {
      id
      imageUrl
      description
    }
  }
`
export default {
  data: () => ({
    description: '',
    imageUrl: '',
  }),

  props: {
      authenticated: {
          type: Boolean
      }
  },

  // Attribute
  methods: {
    create() {
      const description = this.description
      const imageUrl = this.imageUrl

      this.description = ''
      this.imageUrl = ''

      // Mutation
      this.$apollo.mutate({
        mutation: createPost,
        variables: {
          description,
          imageUrl,
        },
      }).then((data) => {
          // Result
          console.log(data)
          this.$router.push({ name: 'Home' });
      }).catch((error) => {
          // Error
          console.error(error)
      })
    },
  },
}
</script>

<style>

.create {
    text-align: center;
    display: flex;
    justify-content: flex-start;
    flex-direction: column;
}

</style>
