<template>
    <div id="project">
        <div id="project_editbtn" v-on:click="projectInteraction('editProject')"><img src="../assets/edit.png"></div>
        <div v-on:click="projectInteraction('open')" id="project_visit_link">
            <div id="project_title">
                <h2>{{ name }}</h2>
            </div>
            <div id="project_description">
                <p>{{ desc }}</p>
            </div>
        </div>
        <div id="project_buttons" v-if="cid===1">
            <button id="project_buttons_left" v-on:click="projectInteraction('delete')" type="button"><img src="../assets/exit.png"></button>
            <button id="project_buttons_right" v-on:click="projectInteraction('moveToFinished')" type="button"><img src="../assets/correct.png"></button>
            <button id="project_buttons_middle" v-on:click="projectInteraction('moveToCancelled')" type="button"><img src="../assets/cancel.png"></button>
        </div>
        <div id="project_buttons" v-if="cid===2">
            <button id="project_buttons_left" v-on:click="projectInteraction('delete')" type="button"><img src="../assets/exit.png"></button>
            <button id="project_buttons_right" v-on:click="projectInteraction('moveToFinished')" type="button"><img src="../assets/correct.png"></button>
            <button id="project_buttons_middle" v-on:click="projectInteraction('moveToInUse')" type="button"><img src="../assets/retake.png"></button>
        </div>
        <div id="solobutton" v-if="cid===3">
            <button id="project_buttons_left" v-on:click="projectInteraction('delete')" type="button"><img src="../assets/exit.png"></button>
        </div>
    </div>
</template>

<script>
export default {
    name: "project",
    props: ['cid', 'id', 'name', 'desc', 'tasks'],
    methods: {
        projectInteraction(message){
            const info = {'cid': this.cid, 'id': this.id, 'name': this.name, 'desc': this.desc,'tasks': this.tasks}
            switch (message) {
                case 'open':
                    this.$emit('event_from_child', 'openProject', info)
                    break;
                case 'editProject':
                    this.$emit('event_from_child', 'editProject', info)
                    break;
                case 'delete':
                    this.$emit('event_from_child', 'deleteProject', info)
                    break;
                case 'moveToFinished':
                    this.$emit('event_from_child', 'moveFinished', info)
                    break;
                case 'moveToCancelled':
                    this.$emit('event_from_child', 'moveCancelled', info)
                    break;
                case 'moveToInUse':
                    this.$emit('event_from_child', 'moveToInUse', info)
                    break;
            }
        }
    }
}
</script>
<style scoped>
#project {
    position: relative;
    margin-left: 10px;
    margin-top: 10px;
    position: relative;
    width: 200px;
    border-radius: 5px;
    border: 1px solid #DDDDDD;
}

@media (max-width: 460px) {
    #project {
        margin-left: 0;
        margin-top: 10px;
        width: 300px;
    }
}

#project_visit_link {
    height: 200px;
    cursor: pointer;
}

#project_title {
    text-align: center;
}

#project_description {
    padding-left: 10px;
    padding-right: 10px;
}

#project_buttons {
    bottom: 0px;
    position: absolute;
    display: flex;
    width: 100%;
    justify-content: center;
    flex-direction: row;
}

#project_buttons > button {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 35px;
    border: 0px;
    width: 50%;
    color: white;
    cursor: pointer;
}

#solobutton > button {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 35px;
    border: 0px;
    width: 100%;
    color: white;
    cursor: pointer;
}

#project_buttons_left > img, #project_buttons_right > img, #project_buttons_middle > img {
    height: 50%;
}

#project_buttons_left {
    background-color: #e84118;
}

#project_buttons_left:hover {
    background-color: #c23616;
}

#project_buttons_right {
    background-color: #00a8ff;
}

#project_buttons_right:hover {
    background-color: #0097e6;
}

#project_buttons_middle {
    background-color: #7f8fa6;
}

#project_buttons_middle:hover {
    background-color: #718093;
}

button:focus {
    outline:none;
}

#project_editbtn {
    position: absolute;
    top: 5px;
    right: 5px;
    height: 20px;
    width: 20px;
}

#project_editbtn > img {
    height: 100%;
}

</style>