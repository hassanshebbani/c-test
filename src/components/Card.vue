<script setup lang="ts">
interface Post {
  id: string;
  date: string;
  postLink: string;
  title: string;
  imageLink?: string;
  imageURL: string;
  authorName: string;
  authorLink: string;
}
const props = defineProps<Post>();
</script>

<template>
  <div class="col-4 p-card">
    <!-- Card Header -->
    <header class="p-card__header">
      <h5 class="p-heading--5">{{ title.split(" ")[0].replace(",", "") }}</h5>
    </header>
    <div class="p-card__content">
      <!-- Card Image -->
      <a :href="imageLink">
        <img alt="" :src="imageURL" />
      </a>
      <!-- Card Title -->
      <h3 class="p-heading--4">
        <a :href="postLink">{{ title }}</a>
      </h3>
      <!-- Card Credits -->
      <p>
        <em
          >by
          <a :href="authorLink">{{ authorName }}</a>
          on {{ date }}</em
        >
      </p>
    </div>
    <!-- Card Footer -->
    <p class="p-card__footer">Article</p>
  </div>
</template>

<style lang="scss" scoped>
.p-card {
  padding: 0 0.5rem 0.5rem;
  border-top: 0.2rem solid purple;
  box-shadow: 0 0 0.5rem rgba(0, 0, 0, 0.2);
  border-radius: 0.25rem;
  .p-card__header {
    padding: 0;
    h5 {
      padding: 1rem 0.5rem 0.5rem;
    }
  }
  .p-card__content {
    padding: 1rem 0.5rem;
    a {
      font-weight: 400;
    }
    em {
      font-weight: 400;
    }
  }
  .p-card__footer {
    border-top: 0.05rem solid rgba(0, 0, 0, 0.15);
    padding: 0.5rem 0.5rem 0;
  }
}
</style>
