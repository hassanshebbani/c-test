<script setup lang="ts">
/* 
* Hassan Shebbani
*/


import { ref, onMounted } from "vue";
import axios from "axios";
import { Post } from "./ts/interfaces";
import CardVue from "./components/Card.vue";

const posts = ref<Post[]>([]);

// Construct Date
const constructDate = (date: string) => {
  const d = date.split("T")[0];
  const month = new Date(d).toLocaleString("default", { month: "long" });
  // 0 => year , 1 => month (long), 2 => day (numeric)
  return `${d.split("-")[2]} ${month} ${d.split("-")[0]}`;
};

// Retrieve the posts and add them to the array to be passed as props for rendering via the card component
const getPosts = async () => {
  try {
    const response = await axios.get(
      "https://people.canonical.com/~anthonydillon/wp-json/wp/v2/posts.json"
    );
    if (response.status === 200) {
      response.data.forEach((post: any, i: number) => {
        const p: Post = {
          id: post["id"],
          date: constructDate(post["date"]),
          postLink: post["link"],
          title: post["title"]["rendered"],
          imageURL: post["featured_media"],
          authorName: post["_embedded"]["author"][0]["name"],
          authorLink: post["_embedded"]["author"][0]["link"],
        };
        //
        // The last post has no "wp:featuredmedia" to get the image link for the anchor tag
        //
        if (post["_embedded"]["wp:featuredmedia"]) {
          p.imageLink = post["_embedded"]["wp:featuredmedia"][0]["link"];
        } else {
          p.imageLink = "#";
        }
        posts.value!.push(p);
      });
    }
  } catch (error) {
    return alert(error);
  }
};
onMounted(async () => {
  getPosts();
});
</script>

<template>
  <main class="main">
    <div class="row">
      <CardVue
        v-for="(item, index) in posts"
        :key="index"
        :id="item.id"
        :date="item.date"
        :postLink="item.postLink"
        :title="item.title"
        :imageLink="item.imageLink"
        :imageURL="item.imageURL"
        :authorName="item.authorName"
        :authorLink="item.authorLink"
      />
    </div>
  </main>
</template>

<style lang="scss" scoped>
.main {
  min-height: 80vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
