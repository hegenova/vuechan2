<template>
  <div class="home">
    <br/>
    <h1>/vue/ vueJS based forum site</h1>
  </div>
   <input type="text" v-model="search" name="search" placeholder="search thread">
   <div class="col-sm-2">
     <form action="#">
      <label for="lang">Sort by </label>
      <select name="languages" id="lang">
        <option value="javascript">Bump</option>
        <option value="javascript">New</option>
        <option value="javascript">Old</option>
      </select>
      <input type="submit" value="Submit" />
</form>
   </div>
  <div class="row">
    <div class="card">
      <div class="card-body" v-for="forum in filteredItems" :key="forum.id">
        <div>
        <img :src="forum.imageThread" onerror="this.style.display='none'"/>
        </div>
        #{{ forum.id}}
        <h5 class="card-title">{{ forum.Title }}</h5>
        <p class="card-text">{{ forum.Description }}
        </p>
        <router-link href="#" class="btn btn-primary" :to="{path:`/thread/${forum.id}`}">view thread</router-link>
        <br />
        last reply:
        <br />
        <div>
        <img :src="forum.imagepost" onerror="this.style.display='none'"/>
        </div>
        <br/>
        {{forum.post}}
        <br/>
        <h6>reply amount: {{forum.postOrder}}</h6>
        ----------------------------------------------------------------
      </div>
    </div>
  </div>
</template>

<style>
.card {
  display: flex;
  align-items: center;
  justify-content: center;
}

.card img {
  height: 200px;
  width: 200px;
  object-fit: cover;
}
</style>

<script>
// @ is an alias to /src
import forColRef from "../firebase";
import { getDocs, getCountFromServer, getDoc, doc, deleteDoc } from "firebase/firestore";
export default {
  name: 'HomeView',
  components: {
  },
  data() {
    return {
      threadCount:"",
      forums: [],
      posts: [],
      lastPost: [],
      numberOfReply: [],
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
    },
    async autoThreadDeletion(){
      let docref = forColRef;
      let ss = await getCountFromServer(docref);
      this.threadCount = ss.data().count;
    }
  },
  created() {
    this.fetchData();

  },
  computed: {
    filteredItems(){
      return this.forums.filter((item) => {
          return item.Title.match(this.search)
      })
    }
  }
}
</script>
