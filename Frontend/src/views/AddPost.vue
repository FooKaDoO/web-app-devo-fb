<template>
  <div class="form">
    <h3>Add a Post</h3>
    <label for="title">Title: </label>
    <input name="title" type="text" id="title" required v-model="post.title" />
    <label for="body">Body: </label>
    <input name="body" type="text" id="body" required v-model="post.body" />
    <label for="urllink">Url: </label>
    <input name="urllink"  type="text" id="urllink" required v-model="post.urllink"/>
    <button @click="addPost" class="addPost">Add Post</button>
  </div>
</template>

<script>
export default {
  name: "AddPost",
  data() {
    return {
      post: {
        title: "",
        body: "",
        urllink: "",
      },
    };
  },
  methods: {
    addPost() {
      var data = {
        title: this.post.title,
        body: this.post.body,
        urllink: this.post.urllink,
      };
      // using Fetch - post method - send an HTTP post request to the specified URI with the defined body
      fetch("http://localhost:3000/api/posts", {
        credentials: 'include',
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(data),
      })
      .then((response) => {
        console.log(response);
        // redirect to /allposts view
        this.$router.push("/");
      })
      .catch((e) => {
        console.log(e);
        console.log("error");
      });
    },
  },
};
</script>

<style scoped>
.form {
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
  align-items: center;
  text-align: center;
}

button:hover{
  transition-duration: 0.25s;
  background: #64E986;
}
</style>