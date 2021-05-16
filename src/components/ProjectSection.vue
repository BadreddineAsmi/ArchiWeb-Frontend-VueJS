<template>
    <div id="projectSection">
        <div id="projectTitleContainer">
            <h1 class="handle">{{ categoryName }}</h1>
        </div>
        <div id="projectContainer" v-if="projects.length > 0">
            <Project @event_from_child="fromProjects" v-for="p in projects" :key="p.id" :cid="cid" :id="p.id" :name="p.name" :desc="p.description" :tasks="p.tasks"></Project>
        </div>
        <div id="projectEmptyContainer" v-else>
            <h3 v-if="cid === 1">Aucun projet en cours</h3>
            <h3 v-if="cid === 2">Aucun projet annulé</h3>
            <h3 v-if="cid === 3">Aucun projet terminé</h3>
        </div>
    </div>
</template>

<script>
import Project from './project'

export default {
    name: "projectSection",
    components: {Project},
    props: ['cid', 'categoryName', 'projects'],
    methods: {
        fromProjects(message, info){
            this.$emit('event_from_child', message, info)
        }
    }
}
</script>
<style scoped>
#projectSection {
    display: flex;
    flex-direction: column;
    margin-top: 20px;
    width: min(90%, 70.5rem);
    background-color: rgba(245, 245, 245, 0.5);
    border: 2px solid #dfe6e9;
}

#projectTitleContainer {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100px;
    width: 100%;
    background-color: rgba(236, 240, 241, 0.5);
    border-bottom: 2px solid #dfe6e9;
}

#projectEmptyContainer {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 80px;
    width: 100%;
}

#projectContainer {
    margin-top: 10px;
    margin-bottom: 10px;
    display: flex;
    justify-content: flex-start;
    flex-wrap: wrap;
    min-height: 200px;
    width: 100%;
}

@media (max-width: 460px) {
    #projectContainer {
        margin-top: 10px;
        margin-bottom: 10px;
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 200px;
        width: 100%;
    }
}

.handle {
    color: #3498db;
}

.canceled {
    color: #e74c3c;
}

.done {
    color: #2ecc71;
}
</style>