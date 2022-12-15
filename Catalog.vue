<template>
  <div class="home">
    <br/>
    <h1>/vue/ vueJS based forum site</h1>
  </div>
   <input type="text" v-model="search" name="search" placeholder="search thread">
  <div class="row">
  <div class="col-sm-6">
    <div class="card">
      <div class="card-body" v-for="forum in forums" :key="forum.id">
        <h5 class="card-title">{{forum.Title}}</h5>
        <div style="max-height:100px; max-width:100px; overflow:hidden"><img :src="forum.image" /></div>
        <p class="card-text">{{forum.Description}}</p>
        <router-link href="#" class="btn btn-primary" :to="{path:`/thread/${forum.id}`}">view thread</router-link>
       
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
  },
  created() {
    this.fetchData();
  }
}
</script>
