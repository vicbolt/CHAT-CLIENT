<template>
    <div class="signup">

        <v-text-field v-model="email" placeholder="Email" type="email" ></v-text-field>
        <v-text-field v-model="password1" placeholder="Contraseña" type="password"></v-text-field>
        <v-text-field v-model="password2" placeholder="Repite la contraseña" type="password"></v-text-field>
        <v-btn block elevation="2" x-large @click="signUp">REGISTRARME </v-btn>
        <div id="enlace" style="text-align: center; margin-top: 13px" >
        <a href="/login"> ¿Ya tienes una cuenta? Inicia sesión para continuar</a>
        </div>


    </div>
</template>


<script>


export default {
    data(){
        return{
            email: "",
            password1:"",
            password2:"",
        }
    },

    methods: {
        async signUp(){

            const body = {
                email: this.email,
                password1: this.password1,
                password2: this.password2,
            }
            
            try {
                const res = await fetch('http://localhost:4500/user/create', {
                method: 'post',
                headers: {
                    'Content-Type' : 'application/json',
                },
                body: JSON.stringify(body),
            })

            const user = await res.json()

            console.log(user)

            if(user.msg){
                alert(user.msg)
                this.$router.push('/login')
            }
            if(user.error){
                alert(user.error)
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