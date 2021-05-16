<template>
    <div id="navbar">
        <div id="nav_conteneur_titre">
            <h1><a id="navbar_titre" href="#">Gestion de projet</a></h1>
        </div>
        <div id="nav_boutons">
            <div v-on:click="createItem" id="nav_creation_projet"></div>
            <div v-on:click="showMenu" id="nav_menu"></div>
        </div>
    </div>
</template>

<script>
export default {
    name: "navbar",
    methods: {
        showMenu(){
            this.$emit('event_from_child', 'openMenu')
        },
        createItem(){
            this.$emit('event_from_child', 'createItem')
        }
    }
}
</script>
<style scoped>
* {
    margin: 0;
}

#navbar {
    height: 60px;
    display: flex;
    flex-direction: row;
    background-color: #ecf0f1;
}

#nav_conteneur_titre {
    padding-top: 8px;
    padding-left: 25px;
    height: 100%;
    width: 95%;
}

#navbar_titre {
    color: #14dcfc;
    text-decoration: none;
}

@media (max-width: 430px) {
    #navbar_titre {
        display: none;
    }
}

#nav_creation_projet {
    height: 100%;
    width: 75px;
    background-image: url("../assets/plus.png");
    background-position: center;
    background-size: 50%;
    background-repeat: no-repeat;
}

#nav_creation_projet:hover {
    background-color: #dcdde1;
    background-size: 75%;
}

#nav_menu {
    height: 100%;
    width: 75px;
    margin-left: 5px;
    background-image: url("../assets/menu.png");
    background-position: center;
    background-size: 50%;
    background-repeat: no-repeat;
}

#nav_menu:hover {
    background-color: #dcdde1;
    background-size: 75%;
}

#nav_boutons {
    display: flex;
    flex-direction: row;
}
</style>