<template>
    <div id="register">
        <label for="prenom"><b>Prénom</b></label>
        <input type="text" v-model="first_name" placeholder="Entrez votre prènom" name="prenom">
        <label for="nom"><b>Nom</b></label>
        <input type="text" v-model="name" placeholder="Entrez votre nom" name="nom">
        <label for="utilisateur"><b>Nom d'utilisateur</b></label>
        <input type="text" v-model="username" placeholder="Entrez votre nom d'utilisateur" name="utilisateur">
        <label for="utilisateur"><b>Addresse e-mail</b></label>
        <input type="text" v-model="email" placeholder="Entrez votre adresse e-mail" name="utilisateur">
        <label for="mdp"><b>Mot de passe: </b></label>
        <input type="password" v-model="password" placeholder="Entrez un mot de passe" name="mdp">
        <button type="submit" v-on:click="validateForm" v-if="this.checkFields()">S'enregistrer</button>
        <button type="submit" v-else>S'enregistrer</button>
        <button type="submit" v-on:click="goback">Retour</button>
    </div>
</template>

<script>
export default {
    name: "register",
    data() { return {
        first_name: '',
        name: '',
        username: '',
        email: '',
        password: ''
    }},
    methods: {
        checkFields(){
            return (this.first_name.length > 0 && this.name.length > 0 && this.username.length > 0 && this.password.length > 0 && this.email.length > 0)
        },
        goback(){
            this.$emit('event_from_child', 'goback')
        },
        validateForm(){
            if(this.checkFields()){
                const info = {'fname': this.first_name, 'name': this.name, 'username': this.username, 'email': this.email, 'password': this.password}
                this.$emit('event_from_child', 'register', info)
            }
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
#register {
    height: 490px;
    width: 400px;
    padding: 10px;
    backdrop-filter: blur(40px);
    border: 3px solid #f1f1f1;
    color: #FFFFFF;
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif
}

@media (max-width: 450px) {
    #register {
        width: 100%;
        padding: 0px;
        padding-bottom: 10px;
        border: 0px;
        backdrop-filter: blur(0px);
    }
    #register > form > label {
        margin-left: 10px;
    }
}
</style>