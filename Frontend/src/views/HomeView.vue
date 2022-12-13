<template>
  <div class="header">
    <div class="container">
    <button v-if = "authResult" @click="Logout" class="center">Logout</button>
    </div>
    <div id="post-list">
    <h1 class="allposts">All Posts</h1>
      <ul>
        <div class="item" v-for="post in posts" :key="post.id">
          <!-- / We are putting an anchor for each post, when we click on it, we will be directed to the specific post view (/apost/) /  -->
          <a class="singlepost" :href="'/api/apost/' + post.id">
            <span class="title"> {{ post.title }} </span><br />
            <span class="body"> {{ post.body }} </span> <br />
            <span class="url"> {{ post.urllink }} </span> <br />
          </a>
        </div>
      </ul>
    </div>
    <div class="add-and-delete-post">
      <button v-if = "authResult" @click="goToAddPost" class="add-button">Add post</button>
      <button v-if = "authResult" @click="DeleteAll" class="delete-button">Delete all</button>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import auth from "../auth";

export default {
  name: "HomeView",
  components: {
  },
   data: function() {
    return {
    posts:[ ],
    authResult: auth.authenticated()
    }
  },
  methods: {
    Logout() {
      fetch("http://localhost:3000/auth/logout", {
          credentials: 'include', //  Don't forget to specify this if you need cookies
      })
      .then((response) => response.json())
      .then((data) => {
        console.log(data);
        console.log('jwt removed');
        //console.log('jwt removed:' + auth.authenticated());
        this.$router.push("/login");
        //location.assign("/");
      })
      .catch((e) => {
        console.log(e);
        console.log("error logout");
      });
    },
    fetchPosts() {
      // You should remember how Fetch API works
      // fetch is a GET request unless stated otherwise. Therefore, it will fetch all posts from the database
      fetch(`http://localhost:3000/api/posts/`, {
        credentials: 'include'
      })
        .then((response) => response.json())
        .then((data) => (this.posts = data))
        .catch((err) => console.log(err.message));
    },
    DeleteAll() {
      fetch(`http://localhost:3000/api/posts/`, {
        credentials: 'include',
        method: "DELETE",
        headers: { "Content-Type": "application/json" },
      })
        .then((response) => {
          console.log(response.data);
          // We are using the router instance of this element to navigate to a different URL location
          this.$router.go();
        })
        .catch((e) => {
          console.log(e);
        });
    },
    goToAddPost() {
      this.$router.push("/api/addpost/");
    }
  },
  mounted() {
    this.fetchPosts(),
    console.log()
    }
};
</script>

<style scoped>
body{
  margin: 20px 40px;
  font-size: 1.2rem;
  letter-spacing: 1px;
  background: #fafafa;
  position: relative;
}
.post-list{
  background: rgb(189, 212, 199);
  margin-bottom: 5px;
  padding: 3px 5px;
  border-radius: 10px;
}
.item{
  padding-left: 15%;
  padding-right: 15%;
  margin-left: 31%;
  margin-right: 31%;
  margin-top: 1%;
  border-radius: 10px;
  background-color: #98AFC7;
  text-align: center;
}
h3{
    margin: 0;
  padding: 0;
  font-family: 'Quicksand', sans-serif;
  color: #444;
  background: #7e9756;
}
p{
  background: #796dbd;
}
h1, h2, h3, h4, ul, li, a, input, label, button, div, footer{
  margin: 0;
  padding: 0;
  font-family: 'Quicksand', sans-serif;
  color: #2c3e50;
  margin-bottom: 1%
}
nav{
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-bottom: 80px;
}
input{
  padding: 10px 12px;
  border-radius: 4px;
  border: 1px solid #ddd;
  font-size: 1em;
  width: 100%;
}
label{
  display: block;
  margin: 20px 0 10px;
}
button{
  margin-top: 30px;
  border-radius: 36px;
  background: #2c3e50;
  border:0;
  font-weight: 700;
  font-size: 0.8em;
  display: block;
  padding: 10px 16px;
  letter-spacing: 2px;
  color: #F5FFFA;
}

nav{
  display: flex;
  align-items: center;
}
.post {
    width: 80%;
    position: relative;
    padding: 10px;
    margin: 10px auto;
    border: 1px solid gray;
    text-align: left;
    background: #796dbd;
}
.center {
  margin: auto;
  border: 0;
  padding: 10px 20px;
  margin-top: 20px;
  margin: 10px auto;
  width: 30%; 
}
.container {
  display: flex;
  justify-content: center;
}
.add-and-delete-post{
  display:flex;
  flex-direction: row;
  margin-left: 31%;
  margin-right: 31%;
  justify-content: space-between;
}

.title{
  font-weight:bold;
  
}
.allposts{
  margin-bottom: 3%
}
a{
  text-decoration: none;
}

a:hover{
  transition-duration: 0.25s;
  color:#F5FFFA
}

.add-button:hover{
  transition-duration: 0.25s;
  background: #64E986;
}
.delete-button:hover{
  transition-duration: 0.25s;
  background: #F75D59;

}
.center:hover{
  transition-duration: 0.25s;
  background: #98AFC7;
}
</style>