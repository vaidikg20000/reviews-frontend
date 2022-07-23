<template>
  <div>
    <v-row>
      <v-col>
        <h2 class="mx-2 my-2">Edit a Review</h2>
      </v-col>
    </v-row>
    <v-form v-model="reviewForm" ref="form">
      <v-row>
        <v-col cols="4">
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
        <v-col cols="4">
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
      <v-col cols="1" class="mx-4"
        ><v-btn error @click="redirectBack">Cancel</v-btn></v-col
      >
      <v-col cols="1">
        <v-btn dark @click="resetForm">Reset</v-btn>
      </v-col>
      <v-col cols="1"
        ><v-btn :disabled="!reviewForm" @click="getReview">save</v-btn></v-col
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
    async getReview() {
      // const newBody = {
      //     title : this.postBody.title,
      //     content : this.postBody.content,
      //     id: id
      // }
      console.log("asfasdsadasds", this.postBody);
      fetch("http://localhost:3000/reviews/" + this.postBody.id, {
        method: "PUT",
        body: JSON.stringify(this.postBody),
        headers: {
          "Content-Type": "application/json",
        },
      })
        .then(async (response) => {
          let res = await response.json();
          if(response.status === 200){
                this.$router.push("/");
          }
        })
        .catch((error) => {
          console.error("Error:", error);
        });
    },
  },
  created() {
    this.postBody.title = this.$route.query.title;
    this.postBody.content = this.$route.query.content;
    this.postBody.id = this.$route.query.id;
  },
};
</script>
