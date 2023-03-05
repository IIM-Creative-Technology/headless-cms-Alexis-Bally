<template>
    <div class="menu">
        <h2 class="menu-title">Portfolio</h2>
        <div class="menu-link">
            <nuxt-link to="/">Retour à l'accueil</nuxt-link>
        </div>
    </div>
    <div class="container">
        <h1 class="title">{{ project.name }}</h1>
        <div class="img-container">
            <img class="img" :src="project.image.url" :alt="project.name">
        </div>
        <p class="description">{{ project.description }}</p>
        <div class="infos">
            <div class="type-container">
                <p class="type">{{ project.project_type.type }}</p>
            </div>
            <div class="techno-container">
                <h3 class="techno-title">Technologies utilisées</h3>
                <div class="techno-img-container">
                    <img v-for="techno in project.technologies" class="techno-img" :src="techno.technologieImage.url" :alt="techno.technologieName">
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>

const { findOne } = useStrapi()
const route = useRoute()

const projectStrapi = await findOne(`projects?filters[slug]=${route.params.slug}&populate=deep`)
var project = projectStrapi.data[0]

</script>

<style scoped>

.menu {
    height: 80px;
    width: 100%;
    color: #8700E8;
    background-color: #181818;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.menu-title {
    font-size: 25px;
    margin-left: 150px;
}

.menu-link {
    margin-right: 150px;
    padding: 10px 15px;
    border: solid 1px white;
    border-radius: 10px;
}

.menu-link a {
    font-size: 18px;
    text-decoration: none;
    color: white;
}

.container {
    background-color: white;
    margin: 0 15%;
}

.title {
    width: fit-content;
    margin: 30px auto;
}

.img-container {
    display: flex;
    justify-content: center;
}

.img {
    max-width: 100%;
    max-height: 50vh;
    margin: auto;
}

.description {
    margin: 25px 0;
}

.infos {
    display: flex;
    justify-content: space-around;
    align-items: center;
    border-top: 1px solid #8700E8;
}

.type-container {
    padding: 10px 25px;
    border-radius: 10px;
    width: fit-content;
    height: fit-content;
    background-color: rgba(135, 0, 232, 100%);
    color: white;
}

.type {
    font-size: 18px;
    font-weight: 700;
}

.techno-container {
    width: fit-content;
    display: flex;
    align-items: center;
    flex-direction: column;
}

.techno-title {
    margin: 20px 0;
}

.techno-img-container {
    display: flex;
    align-items: center;
}

.techno-img {
    width: 100%;
    max-width: 100px;
    max-height: 100px;
    padding: 0 25px;
}

</style>