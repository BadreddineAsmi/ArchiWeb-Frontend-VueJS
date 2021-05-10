<template>
    <div id="application">
        <div id="container">
            <Login @event_from_child="register_event" v-if="login"></Login>
            <Register @event_from_child="goback_event" v-else></Register>
        </div>
        <Foot v-bind:color="footer_color"></Foot>
    </div>
</template>
<script>
import axios from 'axios';
import Login from './components/Login.vue'
import Register from './components/Register.vue'
import Foot from './components/Foot.vue'

export default {
    name: "application",
    components: {Login, Register, Foot},
    data() { return {
        login: true,
        footer_color: '#FFFFFF'
    }},
    methods: {
        register_event(msg, info){
            switch (msg) {
                case 'gotoregister':
                    this.login = false;
                    break;
                case 'login':
                    this.sendCredentials('login', info)
                    break;
            }
        },
        goback_event(msg, info){
            switch (msg) {
                case 'goback':
                    this.login = true;
                    break;
                case 'register':
                    this.sendCredentials('register', info)
                    break;
            }
        },
        sendCredentials(type, info){
            const url = (type === 'register') ? '/api/register' : '/api/login'
            axios.post('http://localhost:8080'+url, info)
            .then(response => { location.href = '/' })
            .catch(error => console.log('Erreur avec l\'API: '+error))
        }
    },
    mounted: () => {
        const element = document.body;
        element.classList.add('bodyparams');
        },
    destroyed() {
        const element = document.body;
        element.classList.remove('bodyparams');
    }
}
</script>
<style scoped>
#logandregister {
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    flex-wrap: wrap;
    align-items: center;
}

#container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 95vh;
    margin: 0;
}
</style>