<template>
    <div id="connexion">
            <div id="userpic">
                <img src="src/assets/userlogo.png" alt="Avatar" class="avatar">
            </div>
            <div>
                <label for="utilisateur"><b>Nom d'utilisateur</b></label>
                <input type="text" v-model="username" placeholder="Entrez votre nom d'utilisateur" name="utilisateur" required>
                <label for="mdp"><b>Mot de passe: </b></label>
                <input type="password" v-model="password" placeholder="Entrez un mot de passe" name="mdp" required>
                <button type="submit" v-on:click="validateForm" v-if="this.username.length > 0 && this.password.length > 0">Se connecter</button>
                <button id="dbutton" type="submit" v-else disabled>Se connecter</button>
                <button type="submit" v-on:click="gotoRegister">Pas de compte?</button>
            </div>
    </div>
</template>

<script>
export default {
    name: "connexion",
    data() { return {
        username: '',
        password: ''
    }},
    methods: {
        gotoRegister(){
            this.$emit('event_from_child', 'gotoregister')
        },
        validateForm(){
            const info = {'username': this.username, 'password': this.password}
            this.$emit('event_from_child', 'login', info)
        }
    }
}
</script>
<style scoped>
input[type=text],
input[type=password] {
    width: 100%;
    padding: 12px 20px;
    margin: 8px 0;
    display: inline-block;
    border: 1px solid #ccc;
    box-sizing: border-box;
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

#dbutton {
    background-color: #c0392b;
}

#dbutton:hover {
    background-color: #e74c3c;
}

#userpic {
    padding-top: 20px;
    padding-bottom: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 250px;
    width: 100%;
}

#userpic>img {
    max-width: 100%;
    max-height: 100%;
}

#connexion {
    height: 550px;
    width: 400px;
    padding: 10px;
    backdrop-filter: blur(40px);
    border: 3px solid #f1f1f1;
    color: white;
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif
}

@media (max-width: 450px) {
    #connexion {
        width: 100%;
        padding: 0px;
        padding-bottom: 10px;
        border: 0px;
        backdrop-filter: blur(0px);
    }
    #connexion_container > label {
        margin-left: 10px;
    }
}
</style>