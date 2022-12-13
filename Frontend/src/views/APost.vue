<template>
  <div class="A Post">
    <div id="form">
      <h3>{{post.title}}</h3>
      <label for="title">Title: </label>
      <input name="type" type="text" id="title" required v-model="post.title" />
      <label for="body">Body: </label>
      <input name="body" type="text" id="body" required v-model="post.body" />
      <label for="url">Url: </label>
      <input name="url" type="text" id="url" required v-model="post.urllink" />
    </div>
    <div class="buttons">
      <button @click="updatePost" class="updatePost">Update Post</button>
      <button @click="deletePost" class="deletePost">Delete Post</button>
    </div>
  </div>
</template>


<script>
export default {
  name: "APost",
  data() {
    return {
      post: {
        id: "",
        title: "",
        body: "",
        urllink: "",
      },
    };
  },
  methods: {
    fetchAPost(id) {
      // fetch one post with the specied id (id)
      fetch(`http://localhost:3000/api/posts/${id}`, {
        credentials: 'include'
      })
        .then((response) => response.json())
        .then((data) => (this.post = data))
        .catch((err) => console.log(err.message));
    },
    updatePost() {
      // using Fetch - put method - updates a specific post based on the passed id and the specified body
      fetch(`http://localhost:3000/api/posts/${this.post.id}`, {
        credentials: 'include',
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(this.post),
      })
        .then((response) => {
          console.log(response.data);
          //this.$router.push("/apost/" + this.post.id);
          // We are using the router instance of this element to navigate to a different URL location
          this.$router.push("/");
        })
        .catch((e) => {
          console.log(e);
        });
    },
    deletePost() {
      // using Fetch - delete method - delets a specific post based on the passed id
      fetch(`http://localhost:3000/api/posts/${this.post.id}`, {
        credentials: 'include',
        method: "DELETE",
        headers: { "Content-Type": "application/json" },
      })
        .then((response) => {
          console.log(response.data);
          // We are using the router instance of this element to navigate to a different URL location
          this.$router.push("/");
        })
        .catch((e) => {
          console.log(e);
        });
    },
  },
  mounted() {
    // call fetchAPost() when this element mounts, and pass to it a route parameter  (id)
    // Route parameters (this.$route.params.id) are named URL segments that are used to capture the values specified at their 
    // position in the URL. The captured values are populated in the req.params object, with the name 
    // of the route parameter specified in the path as their respective keys
    this.fetchAPost(this.$route.params.id);
  },
};
</script>

<style scoped>
#form {
  max-width: 420px;
  margin: 30px auto;
  background: #98AFC7;
  text-align: left;
  padding: 40px;
  border-radius: 10px;
}
h3 {
  text-align: center;
  color: #2c3e50;
}
label {
  color: #F5FFFA;
  display: inline-block;
  margin: 25px 0 15px;
  font-size: 0.8em;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}
input {
  display: block;
  padding: 10px 6px;
  width: 100%;
  box-sizing: border-box;
  border: none;
  border-bottom: 1px solid white;
  color: #2c3e50;
}
button {
  background: #2c3e50;
  border: 0;
  padding: 10px 20px;
  margin-top: 20px;
  color: #F5FFFA;
  border-radius: 20px;
}
.buttons{
  display:flex;
  flex-direction: row;
  margin-left: 36%;
  margin-right: 36%;
  justify-content: space-between;
}

.updatePost:hover{
  transition-duration: 0.25s;
  background: #64E986;
}
.deletePost:hover{
  transition-duration: 0.25s;
  background: #F75D59;

}
</style>