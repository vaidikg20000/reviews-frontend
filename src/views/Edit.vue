<template>
  <div>
    <v-row>
      <v-col>
        <h2 class="mx-2 my-2">Edit a Review</h2>
      </v-col>
    </v-row>
    <v-form v-model="reviewForm" ref="form">
      <v-row>
        <v-col md="4">
          <v-text-field
            label="Title"
            v-model.trim="postBody.title"
            persistent
            class="mx-4"
            prepend-icon="mdi-text"
            :rules="formRules.titleRules"
            required
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col md="4">
          <v-textarea
            class="mx-4"
            :rules="formRules.contentRules"
            label="Content"
            required
            v-model="postBody.content"
          >
          </v-textarea>
        </v-col>
      </v-row>
    </v-form>
    <v-row>
      <v-col md="1" class="mx-4"
        ><v-btn error @click="redirectBack">Cancel</v-btn></v-col
      >
      <v-col md="1">
        <v-btn dark @click="resetForm">Reset</v-btn>
      </v-col>
      <v-col md="1">
        <v-btn color="red" @click="deleteReview()">Delete</v-btn>
      </v-col>
      <v-col md="1"
        ><v-btn :disabled="!reviewForm" @click="updateReview">save</v-btn></v-col
      >
    </v-row>
  </div>
</template>

<script>
export default {
  name: "Edit",
  data() {
    return {
      postBody: {
        title: "",
        content: "",
        id: "",
      },
      reviewForm: false,
      formRules: {
        titleRules: [(v) => !!v || "Title is required"],
        contentRules: [(v) => !!v || "Content is required"],
      },
    };
  },
  methods: {
    redirectBack() {
      this.$router.push("/");
    },
    resetForm() {
      this.$refs.form.reset();
    },
    deleteReview() {
      fetch(
        "https://reviews-app-backend.herokuapp.com/delete/" + this.postBody.id,
        {
          method: "DELETE",
        }
      ).then(async (response) => {
        let res = await response.json();
        if (response.status === 200) {
          this.$router.push("/");
        }
      });
    },
    async updateReview() {
      fetch(
        "https://reviews-app-backend.herokuapp.com/reviews/" + this.postBody.id,
        {
          method: "PUT",
          body: JSON.stringify(this.postBody),
          headers: {
            "Content-Type": "application/json",
          },
        }
      )
        .then(async (response) => {
          let res = await response.json();
          if (response.status === 200) {
            this.$router.push("/");
          }
        })
        .catch((error) => {
          console.error("Error:", error);
        });
    },

    async getReview(id){
      fetch(
        "https://reviews-app-backend.herokuapp.com/reviews/" + id,
        {
          method: "GET",
          headers: {
            "Content-Type": "application/json",
          },
        }
      )
        .then(async (response) => {
          let res = await response.json();
          if (response.status === 200) {
            this.postBody.content = res.content;
            this.postBody.title = res.title;
          }
        })
        .catch((error) => {
          console.error("Error:", error);
        });
    }
  },
  created() {
    this.postBody.id = this.$route.params.id
    this.getReview(this.$route.params.id)
  },
};
</script>
