<template>

    <div class="column is-8-mobile is-3-desktop is-4-tablet">
        <div class="card">
            <div class="card-header">
                <figure class="image">
                    <img v-bind:src="character.image" v-bind:alt="character.name">
                </figure>
            </div>

            <div class="card-content">
                <h3 class="title is-size-4">{{ character.name }}</h3>
                <button class="button is-success is-rounded is-small" @click="showMore(character.id)">
                    Ver más ...
                </button>
            </div>
        </div>
    </div>

</template>

<script>
export default {
    props: [
        'character'
    ],
    methods: {
        showMore(id) {
            this.$emit('showModal', id)
        }
    }
}
</script>

<style lang="css" scoped>
</style>
