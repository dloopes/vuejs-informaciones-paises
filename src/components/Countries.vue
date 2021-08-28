<template>
<div>
  <div class="container">
     <div class="row">
        <div class="col-sm">
        <br>
            <input type="button" lazy class="col"  value="América" @click="(cargarContinente('Americas'))" />
            <input type="button" lazy class="col"  value="Europa" @click="(cargarContinente('Europe'))"/>
            <input type="button" lazy class="col"  value="Ásia" @click="(cargarContinente('Asia'))"/>
            <input type="button" lazy class="col"  value="África" @click="(cargarContinente('Africa'))"/>
            <input type="button" lazy class="col"  value="Oceania" @click="(cargarContinente('Oceania'))"/>
            <input type="button" lazy class="col"  value="Todos" @click="cargarPaises()"/>
            <button onClick="window.location.reload();">Limpar</button>
            <div class="row margintop10">
                <ul v-for="pais in paises" v-bind:key="pais.id">
                    <li><img :src="pais.flag" class="img-fluid img-thumbnail"></li>
                    <h3>Pais:</h3>
                    <li>{{pais.name}}</li>
                    <h3>Populación:</h3>
                    <p :class="[{divClassRed: pais.population > 100000000}, {divClassGreen: true}]">{{pais.population  | separator}}</p>
                    <h3>Capital:</h3>
                    <li>{{pais.capital}}</li>
                    <h3>Región:</h3>
                    <li>{{pais.region}}</li>
                </ul>
                <br>
            </div>
            <div class="row margintop10">
                <ul v-for="continente in continentes_encontratos" v-bind:key="continente.id">
                    <li><img :src="continente.flag" class="img-fluid img-thumbnail"></li>
                    <h3>Pais:</h3>
                    <li>{{continente.name}}</li>
                    <h3>Populación:</h3>
                     <p :class="[{divClassRed: continente.population > 100000000}, {divClassGreen: true}]">{{continente.population  | separator}}</p>
                    <h3>Capital:</h3>
                    <li>{{continente.capital}}</li>
                    <h3>Región:</h3>
                    <li>{{continente.region}}</li>
                </ul>
            </div>
        <div class="container">
          <div class="col-sm">
          <p class="row">Buscar País</p>
            <input type="text" class="col" v-model="buscador" placeholder="Digite...">
            <div class="row margintop10">
                <ul v-for="pais in paises_encontrados" v-bind:key="pais.id">
                    <li><img :src="pais.flag" class="img-fluid img-thumbnail"></li>
                    <h3>Pais:</h3>
                    <li>{{pais.name}}</li>
                    <h3>Populación:</h3>
                     <p :class="[{divClassRed: pais.population > 100000000}, {divClassGreen: true}]">{{pais.population  | separator}}</p>
                    <h3>Capital:</h3>
                    <li>{{pais.capital}}</li>
                    <h3>Región:</h3>
                    <li>{{pais.region}}</li>
                </ul>
             </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'countries',
  data () {
    return {
      buscador: null,
      continente: '',
      pais: '',
      color:'',
      paises: [],
      paises_encontrados: [],
      continentes_encontratos: [],
    }
  },
  methods: {
      cargarPaises(){
        axios.get('https://restcountries.eu/rest/v2/all')
        .then((response) => {
            console.log(response)
            this.paises = response.data;
            },
        error => alert(error));
      },
      cargarContinente(nome_continente){
        const url=`https://restcountries.eu/rest/v2/region/${nome_continente}?fields=name;population;flag;capital;region;`;
        axios.get(url)
        .then((response) => {
            console.log(response);
            this.continentes_encontratos = response.data;
            },
        error => alert(error));
      },
      buscarPais(criterio){
        const url=`https://restcountries.eu/rest/v2/name/${criterio}?fullText=true`;
        axios.get(url)
        .then((response) => {
            console.log(response);
            this.paises_encontrados = response.data;
            },
        error => alert(error));
      },
    },
  watch: {
    buscador: function(criterio){
        //console.log(criterio);
        if (criterio){
            this.buscarPais(criterio);
        }
        else{
            this.paises_encontrados = [];
        }
      },
    },
}
</script>
<style scoped>
h1, h2, h3 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;

}
.margintop10{
    margin-top:10px;
}
.divClassGreen{
    color:green !important;
}
.divClassRed{
  color:red !important;
}
</style>
