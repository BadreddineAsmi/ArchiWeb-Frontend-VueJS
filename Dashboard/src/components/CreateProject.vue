<template> 
    <div id="overlayTask">
        <div id="createProject">
            <div id="closeButton" v-on:click="closeProject"></div>
            <div id="container">
                <label for="nom_projet"><b>Nom du projet</b></label>
                <input type="text" id="input_nom" placeholder="Gestion de projet" name="nom_projet" v-model="projectName">
                <label for="desc"><b>Description du projet</b></label>
                <textarea id="texte_desc" name="desc" placeholder="Description du projet" v-model="projectDesc"></textarea>
                <button id="validateButton" type="button" v-on:click="confirmCreation"><img src="../assets/confirm.png"></button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "createProject",
    props: ['name', 'desc', 'tasks'],
    data() { return {
        projectName: this.name,
        projectDesc: this.desc
    }},
    methods: {
        closeProject(){
            this.$emit('event_from_child', 'closeProject')
        },
        confirmCreation(){
            if(this.projectName.length > 0 && this.projectDesc.length > 0) {
                const info = {'name': this.projectName, 'desc': this.projectDesc}
                this.$emit('event_from_child', 'createProject', info)
                this.closeProject()
            }
        }
    }
}
</script>
<style scoped>
* {
    margin: 0;
}

#overlayTask {
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 10;
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    height: 100vh;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.8);
}

#createProject {
    position: relative;
    min-height: 300px;
    width: 700px;
    background-color: rgba(245, 245, 245, 1);
    border: 2px solid #dfe6e9;
    border-radius: 30px;
}

#container {
    display: flex;
    flex-direction: column;
    padding: 25px;
}

input[type=text],
input[type=password], 
textarea {
    width: 100%;
    padding: 12px 20px;
    margin: 8px 0;
    display: inline-block;
    border: 1px solid #ccc;
    box-sizing: border-box;
}

textarea {
    min-height: 30px;
    max-height: 100px;
    resize: vertical;
}

#closeButton {
    position: absolute;
    top: 15px;
    right: 15px;
    height: 20px;
    width: 20px;
    background: url("../assets/closePopup.png");
    background-size: 100%;
}

#closeButton:hover {
    cursor: pointer;
}

#validateButton {
    height: 50px;
}

#validateButton > img {
    height: 100%;
}

#validateButton {
    background-color: #1abc9c;
}

#validateButton:hover {
    background-color: #16a085;
}

button {
    background-color: #95a5a6;
    color: white;
    padding: 14px 20px;
    margin: 8px 0;
    border: none;
    cursor: pointer;
    width: 100%;
}

button:hover {
    background-color: #7f8c8d;
}

button {
    outline: none;
}
</style>