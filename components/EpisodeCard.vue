<template>
  <div class="w-full md:w-1/2 lg:w-1/3 mb-3 px-2" role="listitem">
    <nuxt-link :to="'/programas/' + episode.id" :key="episode.id" class="hover:shadow-lg hand" tag="div">
      <!-- THUMBNAIL -->
      <div class="w-full rounded overflow-hidden bg-white">
        <img
          ref="thumbnail"
          class="w-full"
          :title="episode.title"
          :alt="'Portada ' + episode.title"
          loading="lazy"
          :srcset="'https://i.ytimg.com/vi_webp/' + episode.link + '/mqdefault.webp 320w, https://i.ytimg.com/vi_webp/' + episode.link + '/maxresdefault.webp 720w'"
          sizes="(max-width: 1000px) 320px, 720px"
        />
        <!-- END THUMBNAIL -->

        <!-- CONTENT -->
        <div
          class="border-r border-b border-l border-gray-400 border-l-0border-gray-400 rounded-b px-2 py-1 flex flex-col justify-between leading-normal"
        >
          <p
            class="text-xl subpixel-antialiased text-red"
          >{{episode.id.toUpperCase()}} - {{episode.title}}</p>
          <div class="inline-block align-top">
            <!-- PEOPLE IN THE EPISODE -->
            <div class="flex flex-inline flex-wrap justify-center">
              <!-- GUESTS -->
              <div v-if="episode.guests && episode.guests.length > 0">
                <p class="text-sm text-gray-900 text-center">Invitad@s</p>
                <div class="flex flex-inline">
                  <Comedian
                    v-for="guest in episode.guests"
                    :key="guest.id"
                    :comedian="guest"
                    :showName="false"
                    :small="true"
                  />
                </div>
              </div>
              <!-- END GUESTS -->

              <!-- SPECIAL MENTION -->
              <div v-if="episode.special != undefined">
                <p class="text-sm text-gray-900">Mención Especial</p>
                <div class="flex flex-inline">
                  <Comedian
                    :key="episode.special.id"
                    :comedian="episode.special"
                    :showName="true"
                    :small="true"
                  />
                </div>
              </div>
              <!-- END SPECIAL MENTION -->
            </div>
            <!-- END PEOPLE IN THE EPISODE -->

            <!-- SECTIONS -->
            <div class="flex flex-inline flex-wrap justify-around align-top">
              <div class="align-top" v-for="section in episode.sections" :key="section.id">
                <span class="text-red align-top text-center text-sm">{{section.name}}</span>
                <div class="flex flex-inline mx-auto">
                  <div
                    v-for="comedian in section.comedians"
                    :key="comedian.id"
                    class="mx-auto w-full"
                  >
                    <Comedian
                      :key="comedian.id"
                      :comedian="comedian"
                      :showName="true"
                      :small="true"
                      :alternateName="section.culture ? section.culture.title : comedian.name"
                    />
                  </div>
                </div>
              </div>
            </div>
            <!-- END SECTIONS -->
          </div>
        </div>
      </div>
      <!-- END CONTENT -->
    </nuxt-link>
  </div>
</template>

<script>
import Comedian from "~/components/Comedian";

export default {
  components: {
    Comedian
  },
  props: ["episode"],
  created() {
    var img = new Image();
    img.onload = () => {
      if (img.height === 90) {
        this.$refs.thumbnail.srcset = this.$refs.thumbnail.srcset.replace(
          "maxresdefault",
          "mqdefault"
        );
      }
    };
    img.src = `https://i.ytimg.com/vi_webp/${this.episode.link}/maxresdefault.webp`;
  }
};
</script>
<style scoped>
.hand {
  cursor: pointer;
}
</style>