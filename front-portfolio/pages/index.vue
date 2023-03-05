<template>
    <div class="menu">
        <h1 class="menu-title">Portfolio</h1>
    </div>
    <div class="all-perso">
        <div class="perso">
            <div class="perso-container">
                <h3>Bonjour, je m'appelle</h3>
                <h2>Alexis Bally</h2>
                <h3>Etudiant à l'IIM</h3>
            </div>
        </div>
        <div class="perso">
            <div class="perso-container">
                <p>Je suis actuellement en 3ème année de Bachelor Coding & Digital Innovation mais aussi en alternance dans l'entreprise Kernix en tant que Chef de projet web.</p>
            </div>
        </div>
    </div>
    <h2 class="project-title">Mes projets</h2>
    <div class="filter">
        <h3>Filter les projets :</h3>
        <button class="filter-button" @click="filterProjects('all')">Tous</button>
        <button class="filter-button" v-for="projectType in types" @click="filterProjects(projectType)">
            {{ projectType }}
        </button>
    </div>
    <div class="all-card">
        <div class="card" v-for="project in filteredProjects">
            <div class="img-container">
                <img class="img" :src="project.image.url" :alt="project.name">
            </div>  
            <div class="text-container">
                <h2 class="title">{{ project.name }}</h2>
                <p class="description">{{ project.description }}</p>
                <div class="type-container">
                    <p class="type">{{ project.project_type.type }}</p>
                </div>
                <nuxt-link class="project-button" :to="'/projects/' + project.slug">
                    Voir le projet
                </nuxt-link>
            </div>
        </div>
    </div>
</template>

<script setup>

const { find } = useStrapi()

const types = ref([])
const activeFilter = ref('all')
const projects = ref()

const filterProjects = (type) => {
    activeFilter.value = type
}

const filteredProjects = computed(() => {
    if(activeFilter.value === 'all') return projects.value
    return projects.value.filter(project => project.type === activeFilter.value)
})

onMounted (async () => {
    projects.value = await find('projects', {populate: 'deep'})
    projects.value = projects.value.data
    types.value = new Set(projects.value.map(project => {
        return project.type
    }))
})

</script>

<style scoped>

.menu {
    height: 80px;
    width: 100%;
    color: #8700E8;
    background-color: #181818;
    display: flex;
    align-items: center;
}

.menu-title {
    font-size: 25px;
    margin-left: 150px;
}

.all-perso {
    display: flex;
    justify-content: space-around;
    align-items: center;
}

.perso {
    width: fit-content;
    max-width: 30%;
    margin: 10%;
    padding: 50px;
    background-color: #7600ca;
    color: white;
    transform: rotate(5deg);
}

.perso-container {
    transform: rotate(-5deg);
}

.perso h2 {
    display: flex;
    margin: 5vh 0;
    font-size: 2em;
}

.perso h3 {
    display: flex;
    font-size: 1.3em;
}

.project-title {
    width: fit-content;
    margin: 20px auto;
}

.filter {
    width: 35%;
    margin-left: 40px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 15px;
}

.filter-button {
    background-color: white;
    border: none;
    font-size: 15px;
    padding: 5px 10px;
}

.filter-button:hover {
    color: #7600ca;
    cursor: pointer;
}

.all-card {
    padding: 50px;
    display: grid;
    column-gap: 50px;
    row-gap: 50px;
    grid-template-columns: 1fr;
}

.card {
    border-radius: 10px;
    border: solid 2px black;
    padding: 20px 30px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.img-container {
    width: 50%;
    padding: 0 1% 0 0;
    border-right: solid 2px #8700E8;
    display: flex;
    justify-content: center;
}

.img {
    max-width: 100%;
    height: auto;
    max-height: 250px;
}

.text-container {
    position: relative;
    width: 50%;
    height: 100%;
    margin: 0 1%;
    padding: 0 10px;
}

.title {
    width: fit-content;
    margin: 0 auto 30px;
}

.description {
    margin-bottom: 40px;
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

.project-button {
    position: absolute;
    right: 10px;
    bottom: 10px;
    color: black;
}

</style>