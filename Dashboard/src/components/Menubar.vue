<template> 
    <div id="menu_container">
        <div id="menu_container_left">
            <div id="menu_container_name">
                <h1>Gestion de projet</h1>
            </div>
            <ul id="menu_container_list">
                <li>Profil</li>
                <li v-on:click="disconnectUser">Déconnexion</li>
                <li v-on:click="closeMenu">Fermer</li>
            </ul>
        </div>        
    </div>
</template>

<script>
export default {
    name: "menu_container",
    methods: {
        closeMenu(){
            this.$emit('event_from_child', 'closeMenu')
        },
        disconnectUser(){
            this.$emit('event_from_child', 'disconnect')
        }
    }
}
</script>
<style scoped>
#menu_container {
    z-index: 10;
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    height: 100vh;
    width: 100%;
}

#menu_container_left {
    height: 100%;
    width: 300px;
    background-color: #ecf0f1;
    border-left: 2px solid #dfe6e9;
    float: right;
}

@media (max-width: 460px) {
    #menu_container_left {
        border: 0px;
        width: 100%;
    }
}

#menu_container_name {
    height: 100px;
    display: flex;
    justify-content: center;
    align-items: center;
}

#menu_container_list {
    padding: 0;
}

#menu_container_list > li {
    display: flex;
    align-items: center;
    padding-left: 10px;
    list-style: none;
    height: 50px;
}

#menu_container_list > li:hover {
    background-color: #bdc3c7;
}
</style>