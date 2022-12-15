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
      <div class="card-body" v-for="forum in forums" :key="forum.id">
        <div style="max-height:100px; max-width:100px; overflow:hidden">
        <img :src="forum.image" />
        </div>
        #{{ forum.id}}
        <h5 class="card-title">{{ forum.Title }}</h5>
        <p class="card-text">{{ forum.Description }}
        </p>
        <router-link href="#" class="btn btn-primary" :to="{path:`/thread/${forum.id}`}">view thread</router-link>
        <br /><br />
        <h5> Last two reply </h5>
        <br/><br/>
        <h5>  Last reply </h5>
        <br/>
        <h5>reply amount</h5> <h5>image amount</h5>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import forColRef from "../firebase";
import { getDocs, getCountFromServer, doc, deleteDoc } from "firebase/firestore";
export default {
  name: 'HomeView',
  components: {
  },
  data() {
    return {
      forums: [],
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
  },
  created() {
    this.fetchData();
  },
  computed: {
    filteredItems(){
      return this.tasks.filter((item) => {
          return item.category.match(this.search)
      })
    }
  }
}
</script>
