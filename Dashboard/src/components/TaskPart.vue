<template>
    <div id="taskPart">
            <div class="main_container_top">
                <button class="app_gobackbutton" type="button" v-on:click="goBack"><img src="../assets/left-arrow.png" alt=""></button>
                <div class="main_container_title">
                    <h1 class="handle">{{ projectName }}</h1>
                </div>
            </div>
            
            <div id="main_app" v-if="tasks.length > 0">
                <div id="main_sidemenu">
                    <div id="main_sidemenu_top_container">
                        <button class="app_go_up" type="button" v-on:click="resetDir"><img src="../assets/folder.png" alt=""></button>
                    </div>
                    <div id="main_sidemenu_bot_container">
                        <ul id="main_sidemenu_bot_items" v-if="directory_selected">
                            <li v-for="task in tasks.find(element => element.catid === directory_id).tasks" v-bind:key="task.id" v-on:click="openTask(task, directory_id)">{{task.name}}</li>
                        </ul>
                        <ul id="main_sidemenu_bot_items" v-else>
                            <li v-for="dir in tasks" v-bind:key="dir.catid" v-on:click="openDir(dir.catid)"><b>{{dir.category}} ({{dir.tasks.length}})</b></li>
                        </ul>
                    </div>
                </div>
                <div id="main_content">
                    <div id="main_content_value_filled" v-if="task_selected">
                        <div id="main_content_value_filled_topside">
                            <h1 id="editFromText" v-on:click="editTask('editTask')">{{task_name}}</h1>
                            <div id="app_button_container" v-if="catid === 1">
                                <button id="app_cancel" type="button"  v-if="task_cat !== 2" v-on:click="editTask('cancelTask')">Annulé</button>
                                <button id="app_delete" type="button" v-on:click="editTask('deleteTask')">Supprimer</button>
                                <button id="app_finished" type="button" v-if="task_cat !== 3" v-on:click="editTask('moveToFinish')">Finir</button>
                                <button id="app_inuse" type="button" v-if="task_cat !== 1" v-on:click="editTask('moveToInUse')">Reprendre</button>
                            </div>
                        </div>
                        <div id="main_content_value_filled_botside">
                            <p>{{ task_content }}</p>
                        </div>
                    </div>
                    <div id="main_content_value_empty" v-else>
                        <img src="../assets/empty.png" />
                        <p>Veuillez cliquer sur une tâche!</p>
                    </div>
                </div>
            </div>
            <div class="main_container_empty" v-else>
                <h3>Vous n'avez actuellement aucune tâche pour ce projet.</h3>
            </div>
        </div>
</template>

<script>
export default {
    name: "taskPart",
    props: ['catid','projectName', 'tasks'],
    data() { return {
        directory_selected: false,
        directory_id: null,
        task_selected: false,
        task_id: null,
        task_cat: null,
        task_name: '',
        task_content: ''
    }},
    methods: {
        goBack(){
            this.$emit('event_from_child', 'goBackTasks')
        },
        openDir(catid){
            this.directory_id = catid
            this.directory_selected = true
        },
        openTask(task, directory_id){
            this.task_id = task.id
            this.task_name = task.name
            this.task_content = task.description
            this.task_cat = directory_id
            this.task_selected = true
        },
        editTask(msg){
            const info = {'cid': this.task_cat,'id': this.task_id, 'name': this.task_name, 'desc': this.task_content}
            switch (msg) {
                case 'editTask':
                    this.$emit('event_from_child', 'editTask', info)
                    break;
                case 'deleteTask':
                    this.$emit('event_from_child', 'deleteTask', info)
                    break;
                case 'cancelTask':
                    this.$emit('event_from_child', 'cancelTask', info)
                    break;
                case 'moveToFinish':
                    this.$emit('event_from_child', 'moveToFinish', info)
                    break;
                case 'moveToInUse':
                    this.$emit('event_from_child', 'moveToInUse', info)
                    break; 
            }
            this.resetViewer()
        },
        resetDir(){
            this.directory_selected = false;
        },
        resetViewer(){
        this.task_selected = false
        this.task_id = null,
        this.task_name = '',
        this.task_content = ''
        }
    }
}
</script>
<style scoped>
* {
    margin: 0;
}

#taskPart {
    display: flex;
    flex-direction: column;
    margin-top: 20px;
    width: min(90%, 70.5rem);
    background-color: rgba(245, 245, 245, 0.5);
    border: 2px solid #dfe6e9;
}
/* Main continer inner start */
.app_gobackbutton {
    height: 100%;
    width: 100px;
    background: red;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #3498db;
}

.app_gobackbutton:hover {
    background-color: #2980b9;
}

.app_gobackbutton > img {
    height: 35%
}

.main_container_title {
    height: 100%;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}

.main_container_top {
    display: flex;
    align-items: center;
    height: 100px;
    width: 100%;
    background-color: rgba(236, 240, 241, 0.5);
    border-bottom: 2px solid #dfe6e9;
}

.main_container_empty {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 80px;
    width: 100%;
}

#editFromText {
    cursor: pointer;
}

.handle {
    color: #3498db;
}

/* Main container inner end */


/* Main APP start */

#main_app {
    display: flex;
    height: 500px;
    width: 100%;
    background-color: rgba(255, 255, 255, 1)
}

#main_sidemenu {
    display: flex;
    flex-direction: column;
    height: 100%;
    width: max(20%, 18rem);
}

#main_sidemenu_top_container {
    display: flex;
    height: 15%;
    width: 100%;
    justify-content: center;
    align-items: center;
    border-bottom: 2px solid #dfe6e9;
}

button {
    border: 0;
}

#main_sidemenu_top_container > button {
    height: 100%;
    width: 100%;
}

.app_go_up {
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #1abc9c;
}

.app_go_up:hover {
    background-color: #16a085;
}

.app_go_up > img{
    height: 35%;
}

#main_sidemenu_bot_container {
    height: 100%;
    width: 100%;
    overflow-y:auto;
}

#main_content {
    height: 100%;
    width: 100%;
    border-left: 2px solid #dfe6e9;
}

#main_content_value_empty {
    display: flex;
    flex-direction: column;
    height: 100%;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}

#main_content_value_empty > img {
    opacity: 0.2;
    height: 50%;
}

#main_content_value_filled {
    display: flex;
    flex-direction: column;
    height: 100%;
    width: 100%;
}

#main_content_value_filled_topside {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    height: 30%;
    width: 100%;
    border-bottom: 2px solid #dfe6e9;
    overflow: hidden;
}

/* Buttons in tasks */
#app_cancel {
    height: 30px;
    width: 100px;
    border-radius: 15px;
    color: white;
    border: 0px;
    background-color:  #e67e22;
    cursor:pointer;
}

#app_finished {
    height: 30px;
    width: 100px;
    border-radius: 15px;
    color: white;
    border: 0px;
    background-color:  #3498db;
    cursor:pointer;
}

#app_inuse {
    height: 30px;
    width: 100px;
    border-radius: 15px;
    color: white;
    border: 0px;
    background-color:  #1abc9c;
    cursor:pointer;
}

#app_delete {
    height: 30px;
    width: 100px;
    border-radius: 15px;
    color: white;
    border: 0px;
    background-color:  #e74c3c;
    cursor:pointer;
}

#main_content_value_filled_topside > h1 {
    text-align: center;
    align-items: center;
}

#main_content_value_filled_botside {
    padding: 10px;
    height: 70%;
}

.main_sidemenu_button {
    height: 25px;
    width: 100px;
    border-radius: 15px;
    color: white;
    border: 0px;
    background-color:  #1abc9c;
    cursor:pointer;
}

.main_sidemenu_button:hover {
    background-color:  #16a085;
}

#main_sidemenu_bot_items {
    padding: 0;
}

#main_sidemenu_bot_items > li {
    display: flex;
    align-items: center;
    padding-left: 10px;
    list-style: none;
    height: 50px;
}

#main_sidemenu_bot_items > li:hover {
    background-color: #bdc3c7;
}

/* Main APP end */
/* Footer start */

footer {
    margin-top: 25px;
    margin-bottom: 5px;
    display: flex;
    justify-content: center;
    align-items: center;
    color: black;
}

/* Footer end */
/* Generale */

button:focus {
    outline:none;
}

/* Responsive */

@media (max-width: 580px){
    .main_container {
        width: 100%;
        border: 0px;
        border-top: 2px solid #dfe6e9;
        border-bottom: 2px solid #dfe6e9;
    }
    #main_content_value_empty {
        height: 280px;
    }
    #main_content_value_filled_topside {
        height: 250px;
    }
    #main_content {
        border-left: 0px;
        width: 100%;
    }
    #main_sidemenu_bot_container {
        border-bottom: 2px solid #dfe6e9;
    }
    #main_sidemenu_top_container {
        height: 75px;
    }
    #main_sidemenu {
        height: 300px;
        width: 100%;
    }
    #main_app {
        height: 700px;
        flex-direction: column;
    }
}

</style>