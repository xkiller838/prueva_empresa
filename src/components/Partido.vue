<template>
  <div>
    <div class="contenedor-ancho centrar">
      <div class="select">
        <select v-model="liga" @change="informacion_liga()">
          <option value="">Escoja un liga:</option>
          <option
            v-for="ligas in partidos"
            :key="ligas.id"
            v-bind:value="ligas"
          >
            {{ ligas.competition.name }}
          </option>
        </select>
      </div>
    </div>
    <div class="contenedor-ancho centrar">
      <button class="boton" @click="consultar_partidos()">Ligas Ramdom</button>
    </div>

    <div class="contenedor-ancho">
      <main class="grid col-3 med-col-2 peq-col-1 no-margen centrar">
        <div class="card cursor">
          <img :src="thumbnail.thumbnail" @click="ver_video(thumbnail)" />
          <h6 class="no-margen texto-centrado">{{ thumbnail.title }}</h6>
        </div>
      </main>
    </div>

    <!-- modal -->
    <transition name="fade">
    <div class="modal" v-if="show">
      <div class="modal__backdrop" @click="closeModal()"/>

      <div class="modal__dialog">
        <div class="modal__header" v-html="video">
          
          <slot name="header"/>
          <button type="button" class="modal__close" @click="closeModal()">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 352 512">
              <path
                fill="currentColor"
                d="M242.72 256l100.07-100.07c12.28-12.28 12.28-32.19 0-44.48l-22.24-22.24c-12.28-12.28-32.19-12.28-44.48 0L176 189.28 75.93 89.21c-12.28-12.28-32.19-12.28-44.48 0L9.21 111.45c-12.28 12.28-12.28 32.19 0 44.48L109.28 256 9.21 356.07c-12.28 12.28-12.28 32.19 0 44.48l22.24 22.24c12.28 12.28 32.2 12.28 44.48 0L176 322.72l100.07 100.07c12.28 12.28 32.2 12.28 44.48 0l22.24-22.24c12.28-12.28 12.28-32.19 0-44.48L242.72 256z"
              ></path>
            </svg>
          </button>
        </div>

        <div class="modal__body">
          <slot name="body"/>
        </div>

        <div class="modal__footer">
          <slot name="footer"/>
        </div>
      </div>
    </div>
  </transition> 

  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      thumbnail: "",
      video: "",
      liga: "",
      partidos: [],
      show: false,
    };
  },

  mounted() {
    this.consultar_partidos();
  },

  methods: {
    consultar_partidos() {
      axios
        .get("https://www.scorebat.com/video-api/v1/")
        .then((response) => {
          this.partidos = response.data
            .sort(() => Math.random() - Math.random())
            .slice(0, 5);
          // console.log(this.partidos);
        })
        .catch((error) => {
          console.log(error);
        });
    },

    informacion_liga() {
      this.thumbnail = this.liga;

      console.log(this.thumbnail);
    },

    ver_video(thumbnail) {
      // console.log(thumbnail.videos[0].embed);

      this.video = thumbnail.videos[0].embed;

      console.log(this.video);
      this.show = true;
      document.querySelector("body").classList.add("overflow-hidden");
    },

    closeModal() {
      this.show = false;
      document.querySelector("body").classList.remove("overflow-hidden");
    },

    openModal() {
      this.show = true;
      document.querySelector("body").classList.add("overflow-hidden");
    },
  },
};
</script>

<style lang="css" scoped>
.select {
  position: relative;
  border: 1px solid #ccc;
  width: 50%;
  overflow: hidden;
  background-color: #fff;
}

.select:before {
  content: "";
  position: absolute;
  right: 5px;
  top: 7px;
  width: 0;
  height: 0;
  border-style: solid;
  border-width: 7px 5px 0 5px;
  border-color: #000000 transparent transparent transparent;
  z-index: 5;
  pointer-events: none;
}

.select select {
  padding: 5px 8px;
  width: 100%;
  border: none;
  box-shadow: none;
  background-color: transparent;
  background-image: none;
  appearance: none;
}

.cursor {
  cursor: pointer;
}


@media screen and (max-width: 576px) {
  .select {
    position: relative;
    border: 1px solid #ccc;
    width: 100%;
    overflow: hidden;
    background-color: #fff;
  }
}

</style>