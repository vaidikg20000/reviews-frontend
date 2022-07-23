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
            <v-textarea class="mx-4" :rules="formRules.contentRules" label="Content" required v-model="postBody.content"> </v-textarea>
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
          ><v-btn :disabled="!reviewForm" @click="saveNewReview"
            >save</v-btn
          ></v-col
        >
      </v-row>
  </div>
</template>

<script>
export default {
  name: "Edit",
  data() {
    return {
        postBody:{
            title:"",
            content:""
        },
        reviewForm:false,
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
    resetForm(){
        this.$refs.form.reset();
    }
  },
};
</script>
