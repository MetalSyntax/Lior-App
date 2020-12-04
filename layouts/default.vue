<template>
  <div>
    <Navbar />
    <Nuxt />
  </div>
</template>

<script>
import Navbar from "../components/Navbar";
export default {
  head() {
    return {
      link: [
        {
          rel: "canonical",
          href: "https://lior-app.vercel.app"
        }
      ]
    };
  }
};
</script>

<style></style>
