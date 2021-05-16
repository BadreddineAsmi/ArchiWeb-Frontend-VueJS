<template>
    <div id="ConnectPart">
        <div id="container">
            <Login @event_from_child="register_event" v-if="login"></Login>
            <Register @event_from_child="goback_event" v-else></Register>
        </div>
        <Foot v-bind:color="footer_color"></Foot>
    </div>
</template>

<script>
import Foot from './Foot.vue'
import Login from './Login.vue'
import Register from './Register.vue'

export default {
    name: "ConnectPart",
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
            this.$emit('event_from_child', type, info)
        }
    }
}
</script>
<style scoped>
* {
    margin: 0;
}

#ConnectPart {
    display: flex;
    flex-direction: column;
    justify-content: center;
    flex-wrap: wrap;
    align-items: center;
    z-index: 10;
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    height: 100vh;
    width: 100%;
    background-image: url('../assets/bg_connect.jpg');
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
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