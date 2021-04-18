<template>
  <div class="bg-blue-900">
    <div class="review">
      <h1
        class="pt-5 text-white text-5xl text-center font-black tracking-widest"
      >
        Restaurant Review
      </h1>
      <form class="pt-5 flex justify-center" @submit="submitForm">
        <div class="flex-row space-y-5">
          <base-card class="w-96">
            <div class="p-3 space-y-2">
              <label class="label font-bold" for="name">Your Name: </label>
              <input
                placeholder="Type Your Name"
                class="input rounded-md py-2 px-3 font-medium placeholder-gray-400 focus:outline-none focus:bg-white"
                id="name"
                type="text"
                v-model.trim="inputName"
                required
              />
            </div>
          </base-card>

          <base-card class="w-80">
            <div class="p-3">
              <h2 class="font-bold">Food Quality:</h2>
              <div>
                <input
                  type="radio"
                  name="quality"
                  id="quality-excellent"
                  value="excellent"
                  v-model="quality"
                  required
                />
                <label class="label" for="quality-excellent"> Excellent</label>
              </div>

              <div>
                <input
                  type="radio"
                  name="quality"
                  id="quality-good"
                  value="good"
                  v-model="quality"
                  required
                />
                <label class="label" for="quality-good"> Good</label>
              </div>

              <div>
                <input
                  type="radio"
                  name="quality"
                  id="quality-avg"
                  value="average"
                  v-model="quality"
                  required
                />
                <label class="label" for="quality-avg"> Average</label>
              </div>

              <div>
                <input
                  type="radio"
                  name="quality"
                  id="quality-dissatified"
                  value="dissatified"
                  v-model="quality"
                  required
                />
                <label class="label" for="quality-dissatified">
                  Dissatified</label
                >
              </div>
            </div>
          </base-card>

          <base-card class="w-96">
            <div class="p-3 space-y-2">
              <h2 class="font-bold">Additonal comment:</h2>
              <textarea
                class="bg-gray-100 rounded border border-gray-400 leading-normal resize-none w-full h-20 py-2 px-3 font-medium placeholder-gray-400 focus:outline-none focus:bg-white"
                placeholder="Type Your Comment"
                v-model.trim="inputComment"
                required
              ></textarea>
            </div>
          </base-card>
          <base-card class="w-96">
            <div class="p-3 space-y-2">
              <h2 class="font-bold">Photo review:</h2>
              <img :src="imgSrc" class="h-48 w-56" />
              <input
                type="file"
                class="ml-6 mt-4 w-52 focus:outline-none"
                @change="uploadImg"
              />
            </div>
          </base-card>
          <div class="flex justify-center">
            <base-button
              bgcolor="bg-yellow-400"
              txtcolor="text-white"
              label="Submit"
              class="hover:bg-green-600 p-2 rounded-xl text-lg font-semibold"
            ></base-button>
          </div>
        </div>
      </form>

      <section class="overflow-hidden">
        <div class="container px-5 py-2 mx-auto lg:pt-12 lg:px-32">
          <div class="flex flex-wrap -m-1 md:-m-2">
            <base-card
              class="flex flex-wrap w-1/3 border-8 border-blue-900"
              v-for="rev in review"
              :key="rev.id"
            >
              <div class="w-full p-1 md:p-2">
                <p>Name: {{ rev.name }}</p>
                <p>Food Quality: {{ rev.quality }}</p>
                <p>Additonal comment: {{ rev.comment }}</p>
                <p v-if="rev.img">
                  Photo review: <img :src="rev.img" class="h-48 w-48" />
                </p>
                <button @click="show(rev)">
                  <i class="material-icons m-1 bg-yellow-400"> edit </i>
                </button>
                <button @click="deleteReview(rev.id)">
                  <i class="material-icons m-1 bg-red-600"> delete_forever </i>
                </button>
              </div>
            </base-card>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import BaseCard from "../components/BaseCard.vue";

export default {
  components: {
    BaseCard,
  },
  data() {
    return {
      url: "http://localhost:5000/review",
      edit: false,
      editId: "",
      inputName: "",
      inputComment: "",
      imgSrc: require("../assets/addImg.png"),
      quality: null,
      review: [],
    };
  },
  methods: {
    submitForm() {
      if (
        this.inputName !== "" &&
        this.quality !== null &&
        this.inputComment !== ""
      ) {
        if (this.edit) {
          this.editReview({
            id: this.editId,
            name: this.inputName,
            quality: this.quality,
            comment: this.inputComment,
            img:
              this.imgSrc === require("../assets/addImg.png")
                ? null
                : this.imgSrc,
          });
        } else {
          this.addNewReview({
            name: this.inputName,
            quality: this.quality,
            comment: this.inputComment,
            img:
              this.imgSrc === require("../assets/addImg.png")
                ? null
                : this.imgSrc,
          });
        }
      }
      // clear form
      this.inputName = "";
      this.quality = null;
      this.inputComment = "";
      this.imgSrc = require("../assets/addImg.png");
    },

    uploadImg(event) {
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.onload = (event) => {
        this.imgSrc = event.target.result;
      };
      reader.readAsDataURL(file);
    },

    show(old) {
      this.edit = true;
      this.editId = old.id;
      this.inputName = old.name;
      this.quality = old.quality;
      this.inputComment = old.comment;
      this.imgSrc = old.img === null ? require("../assets/addImg.png") : old.img;
    },
    async editReview(editing) {
      try {
        const res = await fetch(`${this.url}/${editing.id}`, {
          method: "PUT",
          headers: {
            "content-type": "application/json",
          },
          body: JSON.stringify({
            name: editing.name,
            quality: editing.quality,
            comment: editing.comment,
            img: editing.img,
          }),
        });
        const data = await res.json();
        this.review = this.review.map((rev) =>
          rev.id === editing.id
            ? {
                ...rev,
                name: data.name,
                quality: data.quality,
                comment: data.comment,
                img: data.img,
              }
            : rev
        );
        this.edit = false;
        this.editId = "";
        this.inputName = "";
        this.quality = null;
        this.inputComment = "";
        this.imgSrc = require("../assets/addImg.png");
      } catch (error) {
        console.log(`Could not edit! ${error}`);
      }
    },
    async getResult() {
      try {
        const res = await fetch(this.url);
        const data = await res.json();
        return data;
      } catch (error) {
        console.log(`Could not get! ${error}`);
      }
    },
    async deleteReview(deleteId) {
      try {
        await fetch(`${this.url}/${deleteId}`, {
          method: "DELETE",
        });
        this.review = this.review.filter((rev) => rev.id !== deleteId);
      } catch (error) {
        console.log(`Could not delete! ${error}`);
      }
    },
    async addNewReview(newReview) {
      try {
        const res = await fetch(this.url, {
          method: "POST",
          headers: {
            "content-type": "application/json",
          },
          body: JSON.stringify({
            name: newReview.name,
            quality: newReview.quality,
            comment: newReview.comment,
            img: newReview.img,
          }),
        });
        const data = await res.json();
        this.review = [...this.review, data];
      } catch (error) {
        console.log(`Could not save! ${error}`);
      }
    },
  },
  async created() {
    this.review = await this.getResult();
  },
};
</script>