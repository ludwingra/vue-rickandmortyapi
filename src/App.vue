<template>
    <div id="app">

        <div class="hero is-white is-gradient is-bold">

            <div class="hero-body">
                <h1 class="title">
                    <span class="has-text-success">R&M</span>
                    <span class="subtitle"> Personajes</span>
                </h1>

                <div class="field has-addons is-pulled-right">
                    <div class="control">
                        <input
                            v-model="search"
                            type="text"
                            class="input is-rounded"
                            v-on:keyup.enter="searchData"
                        />
                    </div>

                    <div class="control">
                        <button class="button is-success is-rounded" v-on:click="searchData">
                            Buscar
                        </button>
                    </div>

                </div>

            </div>

        </div>

        <div class="container">

            <div class="columns is-desktop is-mobile is-tablet is-multiline is-centered">
                <character v-for="character in characters" :key="character.id"
                            v-bind:character="character"
                                @showModal="showModal"

                ></character>
            </div>

        </div>

        <div class="columns is-desktop is-mobile is-tablet is-multiline is-centered">
            <div class="column is-one-quarter">

                <nav class="pagination is-centered is-rounded" role="navegation" aria-label="pagination">
                    <a class="pagination-previous" v-on:click="changePages( page-1 )">Angterior</a>
                    <a class="pagination-next" v-on:click="changePages( page+1 )">Siguiente</a>

                    <ul class="pagination-list">
                        <li>
                            <a class="pagination-link is-current">{{ page }}</a>
                        </li>
                    </ul>

                </nav>

            </div>
        </div>

        <div class="modal" :class="{ 'is-active': modal }"
                v-if="modal">

            <div class="modal-background" @click="modal = false">

            </div>

            <div class="modal-card">
                <div class="modal-card-head">
                    <p class="modal-card-title">
                        Acerca de: {{ currentCaracter.name }}
                    </p>
                </div>

                <div class="modal-card-body">
                    <div class="columns">
                        <div class="column">
                            <img v-bind:src="currentCaracter.image" v-bind:alt="currentCaracter.name">
                        </div>
                        <div class="column">
                            <p>Genero: <strong>{{ currentCaracter.gender }}</strong></p>
                            <p>Estado: <strong>{{ currentCaracter.status }}</strong></p>
                            <p>Raza: <strong>{{ currentCaracter.species }}</strong></p>
                            <p>Location: <strong>{{ currentCaracter.location.name }}</strong></p>
                            <p>Origen: <strong>{{ currentCaracter.origin.name }}</strong></p>
                            <p>Episodios: <strong>{{ currentCaracter.episode.length }}</strong></p>
                            <p>Fecha creación: <strong>{{ currentCaracter.created }}</strong></p>
                            <p>Tipo: <strong>{{ currentCaracter.type }}</strong></p>
                        </div>
                    </div>

                </div>

                <footer class="modal-card-foot">
                    <button class="button" @click="modal = false">Cerrar</button>
                </footer>

            </div>
        </div>

    </div>
</template>

<script>

import axios from 'axios'

import Character from './components/Character'

export default {
    name: 'App',
    components: {
        Character
    },
    data: () => {
        return {
            characters: [],
            page: 1,
            pages: 1,
            search: '',
            modal: false,
            currentCaracter: {}
        }
    },
    created() {
        this.fetch()
    },
    methods: {
        fetch() {

            const params = {
                page: this.page,
                name: this.search
            }

            let  result = axios
                .get('https://rickandmortyapi.com/api/character/', { params })
                .then((res) => {
                    this.characters = res.data.results
                    this.pages = res.data.info.pages
                    console.log(res.data.results)
                })
                .catch((err) =>{
                    console.log(err)
                })
        },
        changePages(page){
            this.page = page <= 0 || page > this.pages ? this.page : page
            this.fetch()
        },
        searchData() {
            this.page = 1
            this.fetch()
        },
        showModal(id){
            this.fetchOne(id)
        },
        async fetchOne(id){
            let result = await axios
                .get(`https://rickandmortyapi.com/api/character/${id}/`)
            this.currentCaracter = result.data
            this.modal = true

            console.log(this.currentCaracter, "Personaje")
        }
    }
}
</script>

<style>

</style>
