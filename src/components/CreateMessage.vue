<template>
    <div class="container" style="margin-bottom: 30px">
        <form @submit.prevent="createMessage">
            <div class="form-group">
                <input type="text" name="message" class="form-control" placeholder="Escribe mensaje" v-model="newMessage">
                <p class="text-danger" v-if="errorText">{{ errorText }}</p>
            </div>
            <button class="btn btn-primary" style="background-color: #008000" type="submit" name="action">Enviar</button>
        </form>
    </div>
</template>

<script>
import fb from '@/firebase/init';

export default {
    name: 'CreateMessage',
    props: ['name'],
    data() {
        return {
            newMessage: null,
            errorText: null
        }
    },
    methods: {
        createMessage () {
            if (this.newMessage) {
                fb.collection('messages').add({
                    message: this.newMessage,
                    name: this.name,
                    timestamp: Date.now()
                }).catch(err => {
                    console.log(err);
                });
                this.newMessage = null;
                this.errorText = null;
            } else {
                this.errorText = "Se tiene que insertar un mensaje primero!";
            }
        }
    }    
}
</script>

