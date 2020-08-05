<template>
  <div class="contain container">
    <div class="d grid grid-cols-3">
      <div class="a lg:col-span-1 col-span-3 col-span-3">
        <div class="grid grid-cols-8 gap-6">
          <div class="col-start-2 col-span-3">
            <svg
              class="logo"
              version="1.1"
              id="Capa_1"
              xmlns="http://www.w3.org/2000/svg"
              xmlns:xlink="http://www.w3.org/1999/xlink"
              x="0px"
              y="0px"
              viewBox="0 0 512 512"
              style="enable-background:new 0 0 512 512;"
              xml:space="preserve"
            >
              <path
                d="M490.24,113.92c-13.888-24.704-28.96-29.248-59.648-30.976C399.936,80.864,322.848,80,256.064,80
			c-66.912,0-144.032,0.864-174.656,2.912c-30.624,1.76-45.728,6.272-59.744,31.008C7.36,138.592,0,181.088,0,255.904
			C0,255.968,0,256,0,256c0,0.064,0,0.096,0,0.096v0.064c0,74.496,7.36,117.312,21.664,141.728
			c14.016,24.704,29.088,29.184,59.712,31.264C112.032,430.944,189.152,432,256.064,432c66.784,0,143.872-1.056,174.56-2.816
			c30.688-2.08,45.76-6.56,59.648-31.264C504.704,373.504,512,330.688,512,256.192c0,0,0-0.096,0-0.16c0,0,0-0.064,0-0.096
			C512,181.088,504.704,138.592,490.24,113.92z M192,352V160l160,96L192,352z"
              />
            </svg>
          </div>

          <div class="col-span-4 flex flex-col h-31 items-start justify-center">
            <h1 class="text-thirdly tracking-wider -ml-5 text-3xl">FIND</h1>
            <h1 class="text-thirdly tracking-wider -ml-5 text-3xl">SUBTITLE</h1>
          </div>
        </div>

        <div class="px-12 mt-12">
          <label
            class="block uppercase tracking-wide text-thirdly text-xs font-bold mb-2"
            for="grid-first-name"
          >Link Youtube</label>
          <input
            class="link appearance-none block w-full bg-fourtly text-primary rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white"
            id="grid-first-name"
            type="text"
            placeholder="Jane"
            v-model="url"
          />
        </div>
        <div class="px-12">
          <label
            class="block uppercase tracking-wide text-thirdly text-xs font-bold mb-2"
            for="grid-first-name"
          >Find Subtitle</label>
          <div class="relative">
            <input
              class="kataKunci link appearance-none block w-full bg-fourtly text-primary rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white"
              id="grid-first-name"
              type="text"
              placeholder="Please fill out this field"
              v-model="kataKunci"
            />
            <button
              class="clean absolute text-primary text-sm"
              type="button"
              @click="bersihkan"
              v-if="clear"
            >
              <h1 class="text-3xl">x</h1>
            </button>
            <transition name="fade" mode="out-in">
              <div v-if="daftarHasil.length === 0 && kataKunci.length > 0">
                <p class="text-secondary text-sm">
                  No results for '
                  <strong>{{ kataKunci }}</strong>'
                </p>
              </div>
            </transition>
          </div>
        </div>
      </div>
      <!-- content -->
      <div class="b lg:col-span-2 col-span-3 col-span-3 relative">
        <!-- <div class="grid grid-flow-col grid-cols-2 grid-rows-5 gap-4 px-24 py-20"> -->
        <transition name="fade">
          <div v-if="kataKunci.length < 1">
            <img class="home absolute" src="./assets/undraw_web_search_eetr.svg" alt />
          </div>
        </transition>
        <transition name="fade" mode="out-in">
          <div v-if="daftarHasil.length === 0 && kataKunci.length > 0">
            <img class="notFound absolute" src="./assets/undraw_empty_xct9.svg" alt />
          </div>
        </transition>
        <Container tag="transition-group" name="slide">
          <Item v-for="(hasil, index) in daftarHasil" :key="index" :hasil="hasil" :url="url" />
        </Container>

        <!-- <transition-group name="slide">
            <div
              v-for="(hasil, index) in daftarHasil"
              :key="index"
              class="text-sm h-16 bg-thirdly p-3 text-primary rounded-md"
            >
              <span v-html="hasil.text">{{hasil.text}}</span>

              <a :href="`${url}&t=${hasil.start}s`" target="_blank">Youtube</a>
            </div>
        </transition-group>-->
        <!-- </div> -->

        <div class="absolute info flex flex-row" v-if="clear">
          <div class="total text-primary mb-5 mr-1">Total Hasil: {{ paginasi.total }}</div>
          <div class="halaman text-primary mb-5">• Halaman ke: {{ paginasi.page }}</div>
        </div>
        <div class="absolute pagination">
          <button
            class="bg-primary text-white font-bold py-2 px-4 mr-2 rounded pertama"
            @click="navigasi('first')"
            :class="{'cursor-not-allowed opacity-50':!paginasi['first']}"
          >First</button>
          <button
            class="bg-primary text-white font-bold py-3 px-4 mr-2 rounded sebelumnya"
            @click="navigasi('prev')"
            :class="{'cursor-not-allowed opacity-50':!paginasi['prev']}"
          >
            <svg
              version="1.1"
              id="Layer_1"
              xmlns="http://www.w3.org/2000/svg"
              xmlns:xlink="http://www.w3.org/1999/xlink"
              x="0px"
              y="0px"
              width="12px"
              height="12px"
              viewBox="0 0 492 492"
              style="enable-background:new 0 0 492 492;"
              xml:space="preserve"
              fill="white"
            >
              <path
                d="M198.608,246.104L382.664,62.04c5.068-5.056,7.856-11.816,7.856-19.024c0-7.212-2.788-13.968-7.856-19.032l-16.128-16.12
			C361.476,2.792,354.712,0,347.504,0s-13.964,2.792-19.028,7.864L109.328,227.008c-5.084,5.08-7.868,11.868-7.848,19.084
			c-0.02,7.248,2.76,14.028,7.848,19.112l218.944,218.932c5.064,5.072,11.82,7.864,19.032,7.864c7.208,0,13.964-2.792,19.032-7.864
			l16.124-16.12c10.492-10.492,10.492-27.572,0-38.06L198.608,246.104z"
              />
            </svg>
          </button>
          <button
            class="bg-primary text-white font-bold py-3 px-4 mr-2 rounded selanjutnya"
            @click="navigasi('next')"
            :class="{'cursor-not-allowed opacity-50':!paginasi['next']}"
          >
            <svg
              version="1.1"
              id="Capa_1"
              xmlns="http://www.w3.org/2000/svg"
              xmlns:xlink="http://www.w3.org/1999/xlink"
              x="0px"
              y="0px"
              width="12px"
              height="12px"
              viewBox="0 0 451.846 451.847"
              style="enable-background:new 0 0 ;"
              xml:space="preserve"
              fill="white"
            >
              <path
                d="M345.441,248.292L151.154,442.573c-12.359,12.365-32.397,12.365-44.75,0c-12.354-12.354-12.354-32.391,0-44.744
		L278.318,225.92L106.409,54.017c-12.354-12.359-12.354-32.394,0-44.748c12.354-12.359,32.391-12.359,44.75,0l194.287,194.284
		c6.177,6.18,9.262,14.271,9.262,22.366C354.708,234.018,351.617,242.115,345.441,248.292z"
              />
            </svg>
          </button>
          <button
            class="bg-primary text-white font-bold py-2 px-4 mr-2 rounded terakhir"
            @click="navigasi('last')"
            :class="{'cursor-not-allowed opacity-50':!paginasi['last']}"
          >Last</button>
        </div>
      </div>
    </div>

    <main class="container px-16">
      <svg
        class="logo"
        version="1.1"
        id="Capa_1"
        xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        x="0px"
        y="0px"
        viewBox="0 0 512 512"
        style="enable-background:new 0 0 512 512;"
        xml:space="preserve"
      >
        <path
          d="M490.24,113.92c-13.888-24.704-28.96-29.248-59.648-30.976C399.936,80.864,322.848,80,256.064,80
			c-66.912,0-144.032,0.864-174.656,2.912c-30.624,1.76-45.728,6.272-59.744,31.008C7.36,138.592,0,181.088,0,255.904
			C0,255.968,0,256,0,256c0,0.064,0,0.096,0,0.096v0.064c0,74.496,7.36,117.312,21.664,141.728
			c14.016,24.704,29.088,29.184,59.712,31.264C112.032,430.944,189.152,432,256.064,432c66.784,0,143.872-1.056,174.56-2.816
			c30.688-2.08,45.76-6.56,59.648-31.264C504.704,373.504,512,330.688,512,256.192c0,0,0-0.096,0-0.16c0,0,0-0.064,0-0.096
			C512,181.088,504.704,138.592,490.24,113.92z M192,352V160l160,96L192,352z"
        />
      </svg>

      <div class="mb-5">
        <input class="url" type="text" v-model="url" />
      </div>
      <transition name="slide" appear>
        <div class="mb-5">
          <input class type="text" />
          <form class="w-full max-w-sm">
            <div class="flex items-center border-b border-teal-500 py-2">
              <input
                class="kataKunci appearance-none bg-transparent border-none w-full text-gray-700 mr-3 py-1 px-2 leading-tight focus:outline-none"
                type="text"
                placeholder="Jane Doe"
                aria-label="Full name"
                v-model="kataKunci"
              />

              <button
                class="bg-teal-500 flex-shrink-0 text-white hover:bg-teal-600 text-sm -py-1 px-2 rounded-full"
                type="button"
                @click="bersihkan"
                v-if="clear"
              >x</button>
            </div>
          </form>
        </div>
      </transition>
      <!-- <transition name="fade" mode="out-in">
        <div v-if="daftarHasil.length === 0 && kataKunci.length > 0">
          <p>
            No results for '
            <strong>{{ kataKunci }}</strong>'
          </p>
          <img class="notFound absolute" src="./assets/undraw_no_data_qbuo.svg" alt />
        </div>
      </transition>-->

      <div v-if="hasil">
        <div class="total mb-5">Total Hasil: {{ paginasi.total }}</div>
        <div class="halaman mb-5">Halaman ke: {{ paginasi.page }}</div>
      </div>

      <ul class="daftar">
        <transition-group name="slide">
          <li v-for="(hasil, index) in daftarHasil" :key="index" class="mb-3">
            <span v-html="hasil.text"></span>
            <span>{{menit}}</span>
            <a :href="`${url}&t=${hasil.start}s`" target="_blank">Youtube</a>
          </li>
        </transition-group>
      </ul>

      <!-- <transition name="fade">
        <div v-if="kataKunci.length < 1">
          <img class="home" src="./assets/undraw_web_search_eetr copy.svg" alt />
        </div>
      </transition>-->

      <button
        class="bg-green-600 text-white font-bold py-2 px-4 mr-2 -mt-10 rounded pertama"
        @click="navigasi('first')"
        :class="{'cursor-not-allowed opacity-50':!paginasi['first']}"
      >First</button>
      <button
        class="bg-green-600 text-white font-bold py-3 px-4 mr-2 rounded sebelumnya"
        @click="navigasi('prev')"
        :class="{'cursor-not-allowed opacity-50':!paginasi['prev']}"
      >
        <svg
          version="1.1"
          id="Layer_1"
          xmlns="http://www.w3.org/2000/svg"
          xmlns:xlink="http://www.w3.org/1999/xlink"
          x="0px"
          y="0px"
          width="12px"
          height="12px"
          viewBox="0 0 492 492"
          style="enable-background:new 0 0 492 492;"
          xml:space="preserve"
          fill="white"
        >
          <path
            d="M198.608,246.104L382.664,62.04c5.068-5.056,7.856-11.816,7.856-19.024c0-7.212-2.788-13.968-7.856-19.032l-16.128-16.12
			C361.476,2.792,354.712,0,347.504,0s-13.964,2.792-19.028,7.864L109.328,227.008c-5.084,5.08-7.868,11.868-7.848,19.084
			c-0.02,7.248,2.76,14.028,7.848,19.112l218.944,218.932c5.064,5.072,11.82,7.864,19.032,7.864c7.208,0,13.964-2.792,19.032-7.864
			l16.124-16.12c10.492-10.492,10.492-27.572,0-38.06L198.608,246.104z"
          />
        </svg>
      </button>
      <button
        class="bg-green-600 text-white font-bold py-3 px-4 mr-2 rounded selanjutnya"
        @click="navigasi('next')"
        :class="{'cursor-not-allowed opacity-50':!paginasi['next']}"
      >
        <svg
          version="1.1"
          id="Capa_1"
          xmlns="http://www.w3.org/2000/svg"
          xmlns:xlink="http://www.w3.org/1999/xlink"
          x="0px"
          y="0px"
          width="12px"
          height="12px"
          viewBox="0 0 451.846 451.847"
          style="enable-background:new 0 0 ;"
          xml:space="preserve"
          fill="white"
        >
          <path
            d="M345.441,248.292L151.154,442.573c-12.359,12.365-32.397,12.365-44.75,0c-12.354-12.354-12.354-32.391,0-44.744
		L278.318,225.92L106.409,54.017c-12.354-12.359-12.354-32.394,0-44.748c12.354-12.359,32.391-12.359,44.75,0l194.287,194.284
		c6.177,6.18,9.262,14.271,9.262,22.366C354.708,234.018,351.617,242.115,345.441,248.292z"
          />
        </svg>
      </button>
      <button
        class="bg-green-600 text-white font-bold py-2 px-4 mr-2 rounded terakhir"
        @click="navigasi('last')"
        :class="{'cursor-not-allowed opacity-50':!paginasi['last']}"
      >Last</button>
    </main>
  </div>
</template>

<script>
import Container from "./components/Container.vue";
import Item from "./components/Item.vue";
export default {
  data() {
    return {
      url: "https://www.youtube.com/watch?v=klnvttPfOUM",
      kataKunci: "",
      daftarHasil: [],
      clear: false,
      hasil: false,
      b: 0,
      paginasi: {
        first: null,
        last: null,
        prev: null,
        next: null,
        total: 0,
        page: null,
      },
    };
  },
  watch: {
    kataKunci: async function tanganiKataKunci(kataKunci) {
      if (kataKunci && kataKunci.length >= 3) {
        await this.cari(kataKunci, this.url);
        this.hasil = true;
      } else {
        this.bersihkanHasilDanPaginasi();
      }
      if (this.kataKunci == "") {
        this.clear = false;
      } else {
        this.clear = true;
      }
    },
  },
  components: {
    Container,
    Item,
  },
  computed: {
    menit() {
      this.b += this.daftarHasil.start;
      return this.b;
    },
  },
  methods: {
    async cari(kataKunci, url, paginasi) {
      try {
        const respon = await fetch(
          paginasi
            ? paginasi
            : `https://cari-teks-video-api.vercel.app/api/search?q=${kataKunci}&url=${encodeURIComponent(
                url
              )}`
        ).then((_) => (_.ok ? _.json() : []));
        this.daftarHasil = respon.data;
        this.paginasi.first = respon.first;
        this.paginasi.last = respon.last;
        this.paginasi.prev = respon.prev;
        this.paginasi.next = respon.next;
        this.paginasi.total = respon.total;
        this.paginasi.page = respon.page;
      } catch (error) {}
    },
    async navigasi(type) {
      if (!this.paginasi[type]) {
        return;
      }
      await this.cari(this.kataKunci, this.url, this.paginasi[type]);
    },
    bersihkan() {
      this.kataKunci = "";
      this.bersihkanHasilDanPaginasi();
    },
    bersihkanHasilDanPaginasi() {
      this.daftarHasil = [];
      this.paginasi = {
        first: null,
        last: null,
        prev: null,
        next: null,
        total: 0,
        page: null,
      };
    },
  },
};
</script>

<style>
body {
  background-color: rgb(255, 255, 255);
}
.fade-enter {
  opacity: 0;
}
.fade-enter-active {
  transition: opacity 0.8s;
}
.fade-leave {
}
.fade-leave-active {
  transition: opacity 0.8s;
  opacity: 0;
}

.slide-enter {
  opacity: 0;
}
.slide-enter-active {
  animation: slide-in 0.8s ease-out forwards;
  transition: opacity 0.8s;
}
.slide-leave {
  opacity: 0;
}
.slide-leave-active {
  animation: slide-out 0.8s ease-out forwards;
  transition: opacity 0.8s;
  opacity: 0;
  /* position: absolute; */
}
.slide-move {
  transition: transform 0.8s;
}
@keyframes slide-in {
  from {
    transform: translateY(20px);
  }
  to {
    transform: translateY(0);
  }
}
@keyframes slide-out {
  from {
    transform: translateY(0);
  }
  to {
    transform: translateY(20px);
  }
}
.notFound {
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 400px;
  height: 400px;
}
.home {
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 400px;
  height: 400px;
}
.logo {
  fill: #89c9b8;
  width: 120px;
  height: 120px;
}
.pagination {
  left: 50%;
  bottom: 5%;
  transform: translate(-50%, -50%);
}
.info {
  left: 50%;
  bottom: 10%;
  transform: translate(-50%, -50%);
}
mark {
  background-color: #092532;
  border-radius: 3px;
  color: #89c9b8;
}
.list-item {
  display: flex;
  margin-right: 10px;
}
.clean {
  top: -3px;
  right: 10px;
}
.link {
  box-shadow: inset 0 2px 4px 0 rgba(0, 0, 0, 0.5);
}
/* .con {
  position: relative;
}
.pag {
  top: 90vh;
  position: fixed;
}
.mb-5 {
  margin-bottom: 1rem;
}
.mb-3 {
  margin-bottom: 1rem;
} */
</style>
