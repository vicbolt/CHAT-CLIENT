<template>
    <div class="chat" >

        <h1 style="text-align: center"> CHAT </h1>
        
    <div class="container" v-if="userId && paramId" id="container">

        <div class="message" v-for="message in messages" :key="message._id">
            <p v-if="message.userOwner == userId" id="mio"> {{message.text}} </p>
            <p v-else id="noMio"> {{message.text}} </p>
            <p></p>
        </div>
    </div>

    <v-text-field v-model="message" id="campo"> </v-text-field>
    <v-btn @click="enviarMsg" id="boton"> ENVIAR </v-btn>
    </div>
</template>


<script>
export default {
    data (){
        return{
            messages: [],
            paramId: undefined,
            userId: undefined,
            message: "",
            userTwo: undefined,
        }
    },

    async beforeMount(){
        this.paramId = this.$route.params.id
        this.userId = window.localStorage.getItem('userId')

        const token = window.localStorage.getItem("token")
        if(!token){
            this.$router.push('/login')
        }

        await this.getMessages(token)
    },

    methods: {


        async enviarMsg(){
            try{

                const userOneId = window.localStorage.getItem('userId')
                const userTwoId = this.$route.params.id
                const token = window.localStorage.getItem('token')

                const body = JSON.stringify({
                    userOneId,
                    userTwoId,
                    userOwnerId: userOneId,
                    text: this.message,
                })

                const res = await fetch('http://localhost:4500/message/create',{
                    method: 'post',
                    headers: {
                        'Content-Type': 'application/json',
                        token,
                    },
                    body
                })

                const data = await res.json()

                if(data.error){
                    alert(data.error)
                } else {
                    this.messages = []
                    this.message = ""
                    await this.getMessages(token)

                }

            }catch(error){
                alert(error)
            }
        },


        async getMessages(token){
            try{

                const userOneId = window.localStorage.getItem('userId')
                const userTwoId = this.$route.params.id

                console.log({ userOneId, userTwoId })

                const body = JSON.stringify({
                    userOneId,
                    userTwoId,
                })

                const res = await fetch('http://localhost:4500/message/chat', {
                    method: 'post',
                    headers:{
                        'Content-Type': 'application/json',
                        token,
                    },
                    body,
                })

                const data = await res.json()
                if(data.error){
                    alert(data.error)
                    this.$router.push('/login')
                }
                console.log({ data })

                const messages = data.messages
                for(let i = 0; i < messages.length; i++){
                    this.messages.push(messages[i])
                }


            }catch(error){
                alert(error)
            }
        }
    }
}
</script>


<style scoped>

#container{
    
    margin-top: 13px;
    margin-bottom: 13px;
    background-color: rgb(41, 41, 41);
    border-radius: 15px;
    width: 700px;
    height: 500px;
    overflow-y: scroll
}

#boton{
    
    width: 1160px;

}



#mio{
    position: relative;
    left: 500px;
    width: fit-content;
    padding: auto;
    font-size: medium;
    background-color: rgb(108, 133, 107);
    text-align: right;
    border: 15px solid rgb(108, 133, 107) ;
    border-radius: 25px;

    
}

#noMio{
    position: relative;

    width: fit-content;
    text-align: left;
    font-size: large;
    background-color: rgb(161, 190, 158);
    border: 15px solid rgb(161, 190, 158) ;
    border-radius: 25px;
    
}


</style>