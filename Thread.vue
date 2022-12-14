<template>
  <div class="home">
    <br/>
    <h1>/vue/ vueJS based forum site</h1>
  </div>
  THIS IS THREAD
  <br/>
  <router-link type="submit" class="btn btn-primary" :to="{path: '/thread/newpost'}">new post</router-link>
  <div class="row">
  <div class="col-sm-6">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">title</h5>
        <p class="card-text">description
        </p>
        <router-link type="submit" class="btn btn-success" :to="{path: '/thread/replyPost'}">reply</router-link>
      </div>
    </div>
    <div class="card">
      <div class="card-body">
        <p class="card-text">post
        </p>
        <button type="submit" class="btn btn-success">Reply</button>
      </div>
    </div>
    <div class="card">
      <div class="card-body">
        (You)
        <p class="card-text">post
        </p>
        <button type="submit" class="btn btn-success">Reply</button>
      </div>
    </div>
    <div class="card">
      <div class="card-body">
        <p class="card-text">
            >>post ID
            <br/>
            Reply
        </p>
        <button type="submit" class="btn btn-success">Reply</button>
      </div>
    </div>
    <div class="card">
      <div class="card-body">
          (You)
          <br/>
          >>post ID
        <p class="card-text">Reply
        </p>
        <button type="submit" class="btn btn-success">Reply</button>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
// @ is an alias to /src
import forColRef from "../firebase";
import { getDocs, doc, deleteDoc } from "firebase/firestore";

export default {
  name: 'HomeView',
  components: {
  },
  data() {
    return {
      forums: [],
      selectedDoc: null,
      search: "",
      adminMode: false,
    }
  },
  methods: {
    async fetchData() {
      let dataSS = await getDocs(forColRef);
      let forums = [];
      dataSS.forEach((forum) => {
        let forumData = forum.data();
        forumData.id = forum.id;
        forums.push(forumData);
      });
      this.forums = forums
    },
    async deleteForum(forumID) {
      let forumRef = doc(forColRef, forumID);
      await deleteDoc(forumRef);
      this.$router.go();
    }
  }
}
</script>
