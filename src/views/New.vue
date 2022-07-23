<template>
  <div>
    <v-row>
      <v-col>
        <h2 class="mx-2 my-2">Add a new Review</h2>
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
            required
            label="Content"
            v-model="postBody.content"
          >
          </v-textarea>
        </v-col>
      </v-row>
      <v-row>
        <v-col md="1" class="mx-4"
          ><v-btn error @click="redirectBack">Cancel</v-btn></v-col
        >
        <v-col md="1">
          <v-btn dark @click="resetForm">Reset</v-btn>
        </v-col>
        <v-col md="1"
          ><v-btn :disabled="!reviewForm" @click="saveNewReview"
            >save</v-btn
          ></v-col
        >
      </v-row>
    </v-form>
  </div>
</template>

<script>
export default {
  name: "New",
  data() {
    return {
      postBody: {
        title: "",
        content: "",
      },
      reviewForm: false,
      formRules: {
        titleRules: [(v) => !!v || "Title is required"],
        contentRules: [(v) => (!!v && v.length <= 200) || 'Content is required and Content must be 200 characters or less'],
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
    async saveNewReview() {
      fetch("https://reviews-app-backend.herokuapp.com/reviews/new", {
        method: "POST",
        body: JSON.stringify(this.postBody),
        headers: {
          "Content-Type": "application/json",
        },
      })
        .then(async (response) => {
            let res = await response.json();
            if(response.status===200){
                this.$router.push("/")
            }
        })
        .catch((error) => {
          console.error("Error:", error);
        });
    },
  },
};
</script>
