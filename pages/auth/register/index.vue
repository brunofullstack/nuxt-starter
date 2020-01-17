<template>
    <v-layout column justify-center align-center>
        <v-flex xs12 sm10 md10 lg8>
            <v-card class="login-card" height="auto" color="#e8f0ff">
                <v-row no-gutters>
                    <v-col class="d-flex justify-content-center d-xs-none d-sm-none d-md-flex" cols="12" md="8">
                        <img src="~/assets/img/image.png" width="100%" alt="">
                    </v-col>
                    <v-col cols="12" md="4">
                        <v-card class="p-5" flat style="padding: 25px">
                            <v-row class="my-5" align="center" justify="center">
                                <img src="~/assets/img/logo.png" width="70%" alt="">
                            </v-row>
                            <h2 class="my-10 text-center">Crie sua conta</h2>
                            <v-form>
                                <v-text-field prepend-icon="mdi-account"
                                              @input="username_error = false"
                                              v-model="form.username"
                                              name="Username"
                                              label="Usuário"
                                              placeholder="Insira um nome de usuário"></v-text-field>
                                <v-alert v-if="username_error" dense outlined type="error">{{ username_error }}</v-alert>

                                <v-text-field prepend-icon="mdi-email"
                                              @input="email_error = false"
                                              v-model="form.email"
                                              name="e-mail"
                                              label="E-mail"
                                              type="email"
                                              placeholder="Insira seu e-mail"></v-text-field>
                                <v-alert v-if="email_error" dense outlined type="error">{{ email_error }}</v-alert>

                                <v-text-field prepend-icon="mdi-phone"
                                              @input="phone_error = false"
                                              v-model="form.phone"
                                              name="Telefone"
                                              label="Telefone"
                                              type="text"
                                              placeholder="Insira o número do seu celular"
                                              required></v-text-field>
                                <v-alert v-if="phone_error" dense outlined type="error">{{ phone_error }}</v-alert>

                                <v-text-field prepend-icon="mdi-lock"
                                              v-model="form.password"
                                              name="Password"
                                              label="Senha"
                                              type="password"
                                              placeholder="Crie uma senha"></v-text-field>

                                <v-text-field prepend-icon="mdi-lock"
                                              @keyup.enter.native="onSubmit"
                                              v-model="form.password_confirmation"
                                              label="Confirmar Senha"
                                              type="password"
                                              placeholder="Confirmação de senha"></v-text-field>

                                <v-alert border="left"
                                         close-text="Close Alert"
                                         color="info accent-4"
                                         outlined
                                         dismissible
                                >
                                    A senha deve conter ao menos uma letra Maíuscula, uma letra mínuscula, um número e um caracter
                                    especial.
                                </v-alert>

                                <v-card-actions>
                                    <v-btn class="py-7" primary rounded dark large block color="indigo" @click="onSubmit">Registrar</v-btn>
                                </v-card-actions>
                                <footer class="mt-5 pt-12 text-center">
                                    <p>
                                        Já possui cadastro?
                                        <nuxt-link to="/">ACESSE SUA CONTA</nuxt-link>
                                    </p>
                                    <nuxt-link to="/"> ← Voltar para home</nuxt-link>
                                </footer>
                            </v-form>
                        </v-card>
                    </v-col>
                </v-row>
            </v-card>
        </v-flex>
    </v-layout>
</template>

<script>
    export default {
        layout: 'guest',
        auth: false,
        data () {
            return {
                loader: true,
                form: {
                    username: null,
                    email: null,
                    password: null,
                    phone: null,
                    password_confirmation: null
                },
                email_error: false,
                phone_error: false,
                username_error: false,
                sponsor_error: false,
            }
        },
        async mounted() {
            this.loader = false
        },
        methods: {
            async checkEmail() {
                await this.$axios.post('/user/check-email', {
                    email: this.form.email
                }).catch(error => {
                    this.form.email = null
                    this.email_error = error.response.data.message
                })
            },
            async checkUsername() {
                await this.$axios.post('/user/check-username', {
                    username: this.form.username
                }).catch(error => {
                    this.form.username = null
                    this.username_error = error.response.data.message
                })
            },
            async checkPhone() {
                await this.$axios.post('/user/check-phone', {
                    phone: this.form.phone
                }).catch(error => {
                    this.form.phone = null
                    this.phone_error = error.response.message
                })
            },
            async onSubmit() {
                if(!this.isFilled) {
                    this.$toast('Favor preencher todos os dados de cadastro!');
                    return false
                }

                this.loader = true

                this.$toast('Verificando seus dados...');

                await this.$axios.post('/register', this.form)
                    .then(response => {
                        this.$toast.success(response.data.message);
                        this.$router.push('/')
                    })
                    .catch(error => {
                        this.loader.false
                        this.$toast.error(error.response.message)
                    })
            }
        },
        computed: {
            isFilled() {
                return this.form.username && this.form.password && this.form.email && this.form.phone && this.form.password_confirmation
            }
        },
        head() {
            return {
                title: "Criação de conta | " + `${process.env.PROJECT_NAME}`
            }
        }
    }
</script>

<style scoped>

</style>