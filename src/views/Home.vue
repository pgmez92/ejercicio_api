<template>
  <div class="home">
    <select name="categoria" v-model="categoria" @change="cargarObj">
      <option value="people">Personaje</option>
      <option value="films">Película</option>
      <option value="species">Especie</option>
      <option value="planets">Planeta</option>
    </select>
    <select name="obj" v-model="obj" @change="mostrarDatos">
      <option v-for="(item, i) in opt_obj" :key="i" :value="i + 1">
        {{ item.name }}{{ item.title }}
      </option>
    </select>
    <main :style="invisible">
      <div v-if="categoria == 'people'">
        <label><b>Nombre: </b> {{ info.valor.name }}</label>
        <label><b>F.Nacimiento: </b> {{ info.valor.birth_year }}</label>
        <label><b>Altura: </b> {{ info.valor.heigh }} cm</label>
        <label><b>Peso: </b> {{ info.valor.mass }} kg</label>
        <label><b>Género: </b> {{ info.valor.gender }}</label>
      </div>
      <div v-if="categoria == 'films'">
        <label><b>Título: </b> {{ info.valor.title }}</label>
        <label><b>Dirigida por: </b> {{ info.valor.director }}</label>
        <label><b>Producida por: </b> {{ info.valor.producer }}</label>
        <label><b>Fecha de estreno: </b> {{ info.valor.release_date }}</label>
        <label><b>Prólogo: </b> {{ info.valor.opening_crawl }}</label>
      </div>
      <div v-if="categoria == 'species'">
        <label><b>Nombre: </b> {{ info.valor.name }}</label>
        <label><b>Clasificación: </b> {{ info.valor.classification }}</label>
        <label><b>Designación: </b> {{ info.valor.designation }}</label>
        <label><b>Altura media: </b> {{ info.valor.average_height }} cm</label>
        <label
          ><b>Promedio de vida: </b>
          {{ info.valor.average_lifespan }} años</label
        >
        <label><b>Lengua nativa: </b> {{ info.valor.language }}</label>
      </div>
      <div v-if="categoria == 'planets'">
        <label><b>Nombre: </b> {{ info.valor.name }}</label>
        <label
          ><b>Período de rotación: </b>
          {{ info.valor.rotation_period }} h</label
        >
        <label
          ><b>Período orbital: </b> {{ info.valor.orbital_period }} días</label
        >
        <label><b>Diámetro: </b> {{ info.valor.diameter }} km</label>
        <label><b>Clima: </b> {{ info.valor.climate }}</label>
        <label><b>Terreno: </b> {{ info.valor.terrain }}</label>
        <label><b>Agua superficial: </b> {{ info.valor.surface_water }}</label>
        <label><b>Población: </b> {{ info.valor.population }} habitantes</label>
      </div>
      <img :src="foto" />
    </main>
  </div>
</template>

<script>
import { ref, reactive } from "vue";

export default {
  name: "Home",
  props: {},
  setup() {
    let invisible = ref("display:none");
    let categoria = ref("");
    let obj = ref("");
    let opt_categoria = reactive([]);
    let opt_obj = reactive([]);
    let info = reactive({ valor: "" });

    //cargar obj
    function cargarObj() {
      fetch(`https://swapi.dev/api/${categoria.value}/`)
        .then((res) => res.json())
        .then((datos) => {
          opt_obj.splice(0);
          invisible.value = "display:none";
          datos.results.forEach((element) => {
            opt_obj.push(element);
          });
        });
    }
    //cargar datos del obj
    function mostrarDatos() {
      fetch(`https://swapi.dev/api/${categoria.value}/${obj.value}/`)
        .then((res) => res.json())
        .then((datos) => {
          info.valor = datos;
          invisible.value = "display:block";
          console.log(info.valor);
        });
    }

    return {
      info,
      invisible,
      categoria,
      obj,
      opt_categoria,
      opt_obj,
      cargarObj,
      mostrarDatos,
    };
  },
};
</script>
