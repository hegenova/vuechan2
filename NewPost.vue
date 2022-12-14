<template>
  <div class="home">
    <br/>
    <h1>/vue/ vueJS based forum site</h1>
  </div>
  <form>
  <div class="form-group">
    <label for="formGroupExampleInput2">Post</label>
    <input type="text" class="form-control" id="formGroupExampleInput2" placeholder="post">
  </div>
  <div class="mb-3">
  <label for="formFile" class="form-label">Image</label>
  <input class="form-control" type="file" id="formFile">
</div>
  <button type="submit" class="btn btn-primary">Submit new post</button>
</form>
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
