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
        <v-btn
          class="action"
          @click="redirectEdit(item.review_id, item.title, item.content)"
          >Edit</v-btn
        >
        <v-btn class="action" color="red" @click="deleteReview(item.review_id)"
          >Delete</v-btn
        >
      </template>
      <template v-slot:item.created_at="{ item }">
        {{ moment(item.created_at).format("LLL") }}
      </template>
      <template v-slot:item.content="{ item }">
        {{ item.content.length > 25 ? item.content.substring(0,25) + '...' : item.content }}
      </template>
    </v-data-table>
    <v-snackbar v-model="snackbar.visible" :color="snackbar.color">
      {{ snackbar.errMsg }}
      <template v-slot:action="{ attrs }">
        <v-btn
          color="white"
          text
          v-bind="attrs"
          @click="snackbar.visible = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </div>
</template>

<script>
import moment from "moment";

export default {
  name: "Home",
  components: {},
  data() {
    return {
      moment: moment,
      headers: [
        {
          text: "Review ID",
          align: "center",
          value: "review_id",
        },
        {
          text: "Content",
          align: "center",
          value: "content",
        },
        {
          text: "Title",
          align: "center",
          value: "title",
        },
        {
          text: "Date and time",
          align: "center",
          value: "created_at",
        },
        {
          text: "Actions",
          align: "center",
          value: "actions",
        },
      ],
      reviewItems: [],
      snackbar: {
        visible: false,
        errMsg: "",
        color: "",
      },
    };
  },
  methods: {
    async getReviews() {
      fetch("http://localhost:3000/reviews/all", {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
        },
      })
        .then(async (response) => {
          let res = await response.json();
          this.reviewItems = res;
        })
        .catch((error) => {
          console.error("Error:", error);
        });
    },
    deleteReview(id) {
      fetch("http://localhost:3000/delete/" + id, {
        method: "DELETE",
        // headers: {
        //   // "Content-Type": "application/json",
        //   token: token,
        // },
      }).then(async (response) => {
        let res = await response.json();
        if (response.status === 200) {
          this.snackbar.visible = true;
          this.snackbar.color = "green";
          this.snackbar.errMsg = res;
          this.getReviews();
        }
      });
    },
    redirectEdit(id, title, content) {
      const paramsData = {
        id: id,
        title: title,
        content: content,
      };
      this.$router.push({
        path: "edit/" + id,
        query: { id: id, title: title, content: content },
      });
    },
  },
  created() {
    this.getReviews();
  },
};
</script>
<style scoped>
.action {
  margin: 10px;
}
</style>
