<template>
    <v-card class="cardLogin">
        <v-card-title class="title">Bienvenidos</v-card-title>
        <v-card-text>
            <v-row justify="center" align="center">
                <v-col cols="4" align-self="center">
                    <img src="../../assets/images/pipipipi.jpg" class="imgLogin">
                </v-col>
                <v-col cols="8" align-self="center">
                    <v-form ref="formLogin">
                <v-text-field label="Correo Electronico" 
                              placeholder="Correo Electronico"
                              v-model="correoElectronico"
                              :rules="validarCorreo"
                />
                <v-text-field label="Password" 
                              placeholder="Password"
                              v-model="password"
                              :rules="validarPassword"
                />
            </v-form>
                </v-col>
            </v-row>
            
        </v-card-text>
        <v-card-actions>
            <v-btn
            class="btnLogin"
            rounded
            block
            @click="loginBackend"
            >
                Iniciar
            </v-btn>
        </v-card-actions>
    </v-card>
</template>

<script>
export default {
    data() {
        return {
            correoElectronico: '',
            validarCorreo: [
            v => !v || /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid'
            ],
            password:'',
            validarPassword: [
                value => value.length >= 6 || 'Min 6 characters'
            ]
        }
    },
    methods: {
        async loginBackend () {
            const valid = this.$refs.formLogin.validate()
            if (valid) {
                const sendData = {
                    email: this.correoElectronico,
                    password: this.password
                }
                await this.$auth.loginWith('local', {
                    data: sendData
                }).then(async (res) => {
                    console.log('respuesta del back:', res)
                    if (res.data.alert !== 'No se han recibido datos correctamente') {
                        this.$router.push('/dashboard')
                    }
                }).catch((error) => {
                    console.log('error: ', error)
                })
            }else {
                alert('no cumpliste las reglas')
            }
        }
    }
}
</script>

<style scoped>
    .cardLogin{
        background-color: orange;
        border-radius: 10px;
        width: 500px;
        height: 300px;
    }
    .imgLogin {
        width: 100%;
        height: 100%;
    }
    .btnLogin{
        background-color: red!important;
        color: #FFFFFF;
    }
    .title {
        font-size: 30px;
        justify-content: center;
        color: red;
        font-weight: 700;
    }
</style>