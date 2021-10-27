<template>
    <div class="login">

        <v-text-field type="email" v-model="email" placeholder="Email" ></v-text-field>
        <v-text-field type="password" v-model="password" placeholder="Contraseña"></v-text-field>
        <v-btn block elevation="2" x-large @click="login">INICIAR SESIÓN </v-btn>
        <div id="enlace" style="text-align: center; margin-top: 13px" >
        <a href="/signUp"> ¿No tienes una cuenta? Registrate para continuar</a>
        </div>

    </div>
</template>


<script>


export default {
    data(){
        return{
            email: "",
            password:""
        }
    },

    methods: {
        async login(){

            const body = {
                email: this.email,
                password: this.password
            }
            
            try {
                const res = await fetch('http://localhost:4500/user/login', {
                method: 'post',
                headers: {
                    'Content-Type' : 'application/json',
                },
                body: JSON.stringify(body),
            })

            const data = await res.json()

            if(data.error){
                alert(data.error)
                return 
            }
            

            window.localStorage.setItem('token', data.token)
            window.localStorage.setItem('userId', data.userId)

            if(data.token){
                this.$router.push('/users') 
            }
            

            }catch(err){
                alert(err)
            }
        }
    }
}

</script>


<style scoped>


</style>