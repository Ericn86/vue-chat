<template>
    <div class="container chat">
        <h2 class="text-primary text-center">CHAT</h2>
        <div class="card">
            <div class="card-body">
                <div class="messages" v-chat-scroll="{always: false, smooth: true}">
                    <div v-for="message in messages" :key="message.id">
					    <span class="text-secondary time">{{message.date}}{{message.timestamp}}</span>
                        <span class="text-info">{{ message.name }}: </span>
                        <span>{{message.message}}</span>
                    </div>
                </div>
            </div>
            <div class="card-action">
                <CreateMessage :name="name"/>
            </div>
        </div>
    </div>
</template>

<script>
import CreateMessage from '@/components/CreateMessage';
import fb from '@/firebase/init';
import moment from 'moment';

export default {
    name: 'Chat',
    props: ['name'],
    components: {
        CreateMessage
    },
    data() {
        return {
            messages: []
        }
    },
    created() {
        let ref = fb.collection('messages').orderBy('timestamp');

        ref.onSnapshot(snapshot => {
            snapshot.docChanges().forEach(change => {
                if (change.type = 'added') {
                    let doc = change.doc;
                    this.messages.push({
                        id: doc.id,
                        name: doc.data().name,
                        message: doc.data().message,
                        timestamp: moment(doc.data().timestamp).format('LTS'),
						date: doc.date
                    });
                }
            });
        });
    }
}
</script>

<style>
.chat h2{
    font-size: 3em;
    margin-bottom: 10px;
	color: #008000 !important;
	font-weight: bold;
}

.chat span{
    font-size: 1.2em;
}

.chat .time{
    display: block;
    font-size: 0.7em;
}

.messages{
    max-height: 300px;
    overflow: auto;
}
</style>
