<template>
  <div class="home">
    <v-data-table
      :headers="headers"
      :items="reviewItems"
      class="elevation-1"
      hide-default-footer
      disable-pagination
    >
    <template v-slot:item.actions="{ item }">
          <v-btn class="action" @click="redirectEdit()">Edit</v-btn>
          <v-btn class="action"  color="red" @click="deleteSong(item.song_id)">Delete</v-btn>
    </template>
    <template v-slot:item.created_at="{ item }">
          {{ moment(item.created_at).format("LLL") }}
        </template>
    </v-data-table>
  </div>
</template>

<script>
import moment from "moment";

export default {
  name: "Home",
  components: {},
  data(){
    return{
      moment: moment,
      headers: [
          {
            text: 'Review ID',
            align: 'center',
            value: 'review_id',
          },
          {
            text: 'Content',
            align: 'center',
            value: 'content',
          },
          {
            text: 'Title',
            align: 'center',
            value: 'title',
          },
          {
            text: 'Date and time',
            align: 'center',
            value: 'created_at',
          },
          {
            text: 'Actions',
            align: 'center',
            value: 'actions',
          },
      ],
      reviewItems:[],

    }
  },
  methods:{
    async getReviews(){
      fetch("http://localhost:3000/reviews/all", {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
        },
      })
        .then(async (response) => {
            let res = await response.json();
            this.reviewItems = res;
            console.log(res);
            // if(response.status===200){
            //     this.$router.push("/")
            // }
        })
        .catch((error) => {
          console.error("Error:", error);
        });
    }
  },
  created(){
    this.getReviews();  
  }
};
</script>
<style scoped>
.action{
  margin:10px;
}
</style>