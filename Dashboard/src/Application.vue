<template>
    <div id="dashboard">
        <Menubar @event_from_child="openFromMenuBar" v-if="menuBarActive"></Menubar>
        <CreateProject @event_from_child="openFromMenuBar" :name="createProject_name" :desc="createProject_desc" v-if="addProjectActive"></CreateProject>
        <CreateTask @event_from_child="openFromMenuBar" :projectID="project_selected_id" :name="createTask_name" :desc="createTask_desc" :catid="project_category_id" v-else-if="addTaskActive"></CreateTask>
        <Navbar @event_from_child="openFromMenuBar"></Navbar>
        <div id="main_container">
            <section v-if="!project_selected && clientData !== null">
                <ProjectSection @event_from_child="fromProjects" v-for="element in clientData" :key="element.catid" :cid="element.catid" :categoryName="element.category" :projects="element.projects"></ProjectSection>
            </section>
            <section v-else-if="project_selected">
                <TaskPart @event_from_child="fromTasks" :catid="project_category_id" :projectName="project_selected_name" :tasks="project_selected_content"></TaskPart>
            </section>
        </div>
        <Foot v-bind:color="footer_color"></Foot>
    </div>
</template>

<script>
import axios from 'axios';
import Navbar from './components/Navbar.vue'
import TaskPart from './components/TaskPart.vue'
import ProjectSection from './components/ProjectSection'
import Menubar from './components/Menubar.vue'
import CreateProject from './components/CreateProject.vue'
import CreateTask from './components/CreateTask.vue'
import Foot from './components/Foot.vue'

export default {
    name: "dashboard",
    components: {Foot, ProjectSection, TaskPart, CreateProject, CreateTask, Menubar, Navbar},
    data() { return {
        clientData: null,
        menuBarActive: false,
        project_selected: false,
        project_selected_name: '',
        project_selected_id: null,
        project_category_id: null,
        project_selected_content: null,
        addProjectActive: false,
        createProject_id: null,
        createProject_cid: null,
        createProject_name: '',
        createProject_desc: '',
        createTask_id: null,
        createTask_cid: null,
        createTask_name: '',
        createTask_desc: '',
        addTaskActive: false,
        footer_color: '#000000',
        api_URL: 'http://localhost:8080'
    }},
    methods: {
        openFromMenuBar(message, info) {
            switch (message) {
                case 'openMenu':
                    this.menuBarActive = true;
                    break;
                case 'closeMenu':
                    this.menuBarActive = false;
                    break;
                case 'disconnect':
                    this.disconnect()
                    this.menuBarActive = false;
                    break;
                case 'createItem':
                    if(this.project_selected) this.addTaskActive = true;
                    else this.addProjectActive = true;
                    break;
                case 'createProject':
                    if(this.createProject_id === null){
                        this.addProject(info.name, info.desc)
                    } else {
                        this.updateElement('project', this.createProject_id, info.name, info.desc, this.createProject_cid)
                        this.createProject_id = null
                        this.createProject_cid = null
                        this.createProject_name = ''
                        this.createProject_desc = ''
                    }
                    break;
                case 'closeProject':
                    this.addProjectActive = false
                    this.createProject_id = null
                    this.createProject_cid = null
                    this.createProject_name = ''
                    this.createProject_desc = ''
                    break;
                case 'createTask':
                    if(this.createTask_id === null){
                        this.addTask(info)
                    } else {
                        this.updateElement('task', this.createTask_id, info.name, info.desc, this.createTask_cid)
                        this.createTask_id = null
                        this.createTask_cid = null
                        this.createTask_name = ''
                        this.createTask_desc = ''
                    }
                    break;
                case 'closeTask':
                    this.addTaskActive = false;
                    this.createTask_id = null
                    this.createTask_cid = null
                    this.createTask_name = ''
                    this.createTask_desc = ''
                    break;
            }
        },
        fromProjects(msg, info){
            switch(msg){
                case 'openProject':
                    this.project_selected_name = info.name
                    this.project_selected_content = info.tasks.sort(function(a, b){return a.catid - b.catid})
                    this.project_selected_id = info.id
                    this.project_category_id = info.cid
                    this.project_selected = true
                    break;
                case 'editProject':
                    this.addProjectActive = true;
                    this.createProject_id = info.id
                    this.createProject_cid = info.cid
                    this.createProject_name = info.name
                    this.createProject_desc = info.desc
                    break;
                case 'deleteProject':
                    this.deleteProject(info.id)
                    break;
                case 'moveFinished':
                    this.updateElement('project', info.id, info.name, info.desc, 3)
                    break;
                case 'moveCancelled':
                    this.updateElement('project', info.id, info.name, info.desc, 2)
                    break;
                case 'moveToInUse':
                    this.updateElement('project', info.id, info.name, info.desc, 1)
                    break;    
            }
        },
        fromTasks(msg, info){
            switch (msg) {
                case 'goBackTasks':
                    this.project_selected_name = ''
                    this.project_selected_content = null
                    this.project_selected = false
                    this.project_selected_id = null
                    this.project_category_id = null
                    break;
                case 'editTask':
                    this.createTask_id = info.id
                    this.createTask_cid = info.cid
                    this.createTask_name = info.name
                    this.createTask_desc = info.name
                    this.addTaskActive = true
                    break;
                case 'deleteTask':
                    this.deleteTask(info.id)
                    break;
                case 'moveToFinish':
                    this.updateElement('task', info.id, info.name, info.desc, 3)
                    break;
                case 'cancelTask':
                    this.updateElement('task', info.id, info.name, info.desc, 2)
                    break;
                case 'moveToInUse':
                    this.updateElement('task', info.id, info.name, info.desc, 1)
                    break;  
            }
        },
        updatePage(){
            axios.get(this.api_URL+'/api/project')
            .then(response => { 
                this.clientData = response.data.sort(function(a, b){return a.catid - b.catid})
                // On regarde si un projet etais en cours de lecture, cela signifie que une tâche a été modifié!
                if(this.project_category_id !== null){
                    const project_category = this.clientData.find(element => element.catid === this.project_category_id).projects
                    const tasks = project_category.find(element => element.id === this.project_selected_id).tasks
                    this.project_selected_content = tasks.sort(function(a, b){return a.catid - b.catid})
                }
            })
            .catch(error => console.log('Erreur avec l\'API: '+error))
        },
        deleteProject(idproject){
            axios.delete(this.api_URL+'/api/project/'+idproject)
            .then(response => { this.updatePage() })
            .catch(error => console.log('Erreur avec l\'API: '+error))
        },
        addProject(name, desc){
            axios.post(this.api_URL+'/api/project', { name: name, desc: desc })
            .then(response => { this.updatePage() })
            .catch(error => console.log('Erreur avec l\'API: '+error))
        },
        addTask(task){
            axios.post(this.api_URL+'/api/task', task)
            .then(response => { this.updatePage() })
            .catch(error => console.log('Erreur avec l\'API: '+error))
        },
        deleteTask(taskID){
            axios.delete(this.api_URL+'/api/task/'+taskID)
            .then(response => { this.updatePage() })
            .catch(error => console.log('Erreur avec l\'API: '+error))
        },
        updateElement(type, id, name, desc, categoryID){
            const urlAPI = (type === 'project') ? '/api/project' : '/api/task'
            axios.put(this.api_URL+urlAPI, { id: id, name: name, desc: desc, category: categoryID})
            .then(response => { this.updatePage() })
            .catch(error => console.log('Erreur avec l\'API: '+error))
        },
        disconnect(){
            location.href="/"
        }
    },
    mounted: function () {
        const element = document.body;
        element.classList.add('body')
        this.updatePage()
        },
    destroyed() {
        const element = document.body;
        element.classList.remove('body');
    }
}
</script>
<style scoped>
section {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    min-height: 500px;
    width: 100%;
}
</style>