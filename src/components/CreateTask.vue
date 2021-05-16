<template> 
    <div id="overlayTask">
        <div id="createTask">
            <div id="closeButton" v-on:click="closeTask"></div>
            <div id="container" v-if="catid===1">
                <label for="taskname"><b>Nom de la tâche</b></label>
                <input type="text" id="input_nom" placeholder="Manger une pomme..." name="taskname" v-model="taskName">
                <label for="desc"><b>Description de la tâche</b></label>
                <textarea id="texte_desc" name="desc" placeholder="Description de la tâche" v-model="taskDesc"></textarea>
                <button id="validateButton" type="button" v-on:click="createTask"><img src="../assets/confirm.png"></button>
            </div>
            <div id="container" v-else>
                <label for="taskname"><b>Nom de la tâche</b></label>
                <input type="text" id="input_nom" placeholder="Manger une pomme..." name="taskname" disabled>
                <label for="desc"><b>Description de la tâche</b></label>
                <textarea id="texte_desc" name="desc" placeholder="Description de la tâche" disabled></textarea>
                <p class="disabled">Impossible de rajouter une tâche dans ce projet!</p>
                <button id="disabledButton" type="button" disabled><img src="../assets/confirm.png"></button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "createTask",
    props: ['catid', 'projectID', 'name', 'desc'],
    data() { return {
        taskName: this.name,
        taskDesc: this.desc
    }},
    methods: {
        closeTask(){
            this.$emit('event_from_child', 'closeTask')
        },
        createTask(){
            const info = {'id': this.projectID, 'name': this.taskName, 'desc': this.taskDesc}
            this.$emit('event_from_child', 'createTask', info)
            this.closeTask()
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

#createTask {
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
textarea, 
select {
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

#newCategory {
    display: flex;
    flex-direction: column;
    border: 1px solid white;
    padding: 10px;
    width: 100%;
}

#newCategory {
    display: flex;
    flex-direction: column;
    padding: 10px;
    width: 100%;
}

#validateButton {
    height: 50px;
    background-color: #1abc9c;
}

#validateButton > img {
    height: 100%;
}

#validateButton:hover {
    background-color: #16a085;
}

#disabledButton {
    height: 50px;
    background-color: #95a5a6;
}

#disabledButton > img {
    height: 100%;
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

.disabled {
    font-size: 0.7rem;
    color: #e74c3c;
}

</style>