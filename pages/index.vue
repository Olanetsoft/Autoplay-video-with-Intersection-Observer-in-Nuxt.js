<template>
  <div class="container">
    <h1
      class="flex justify-center items-center h-screen space-x-10 px-18 font-bold text-4xl mb-2 justify-center items-center"
    >
      Autoplay video with Intersection Observer in Nuxt.js
    </h1>

    <div class="flex justify-center items-center h-screen space-x-10">
      <div class="rounded overflow-hidden shadow-lg mb-4 content-center">
        <div class="px-6 py-4 pb-2">
          <div class="pb-4 pt-8"></div>

          <video
            id="video-player"
            controls
            muted
            width="800px"
            class="width-full"
            ref="theVideo"
          ></video>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cld: null,
      player: null,
      video: "samples/animals/testvid",
      observer: null,
    };
  },

  mounted() {
    this.cld = cloudinary.Cloudinary.new({
      cloud_name: process.env.NUXT_ENV_CLOUDINARY_CLOUD_NAME,
      secure: true,
    });

    this.player = this.cld.videoPlayer("video-player", {
      analytics: {
        events: ["ended", "play", "pause", "start"],
      },
    });

    this.player.source(this.video);

    this.onElementObserved();
  },

  methods: {
    onElementObserved() {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.intersectionRatio > 0) {
            this.player.play();
          }
        });
      });
      observer.observe(this.$refs.theVideo);
    },
  },

  name: "IndexPage",
};
</script>
