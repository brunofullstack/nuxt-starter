<template>
    <v-layout column justify-center align-center>
        <v-flex xs12 sm10 md10 lg8>
            <v-card class="p-5" flat style="padding: 25px">
                <v-row class="my-5" align="center" justify="center">
                    <img src="~/assets/img/logo.png" width="60%" alt="">
                </v-row>
                <h2 class="my-10 py-5 text-center">Redefinir senha</h2>
                <v-form>
                    <v-text-field prepend-icon="mdi-envelope-o"
                                  @keyup.enter.native="onSubmit"
                                  v-model="email"
                                  name="e-mail"
                                  label="E-mail"
                                  type="email"
                                  placeholder="Insira seu e-mail"></v-text-field>

                    <v-text-field prepend-icon="mdi-lock"
                                  v-model="password"
                                  label="Senha"
                                  type="password"
                                  placeholder="Escolha uma nova senha para sua conta."></v-text-field>

                    <v-text-field prepend-icon="mdi-lock"
                                  @keyup.enter.native="onSubmit"
                                  v-model="password_confirmation"
                                  label="Confirme a Senha"
                                  type="password"
                                  placeholder="Confirme sua nova senha."></v-text-field>

                    <v-card-actions>
                        <v-btn class="py-7" @click="onSubmit" primary rounded dark large block color="indigo">Redefinir Senha</v-btn>
                    </v-card-actions>
                    <footer class="mt-5 pt-10 text-center">
                        <nuxt-link to="/"> ← Voltar para home</nuxt-link>
                    </footer>
                </v-form>
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
                email: null,
                password: null,
                password_confirmation: null,
                token: null,
            }
        },
        async mounted() {
            this.loader = false
            this.token = this.$route.query.token
        },
        methods: {
            loginRoute() {
                this.$router.push('/')
            },
            async onSubmit() {
                if (!this.email||!this.password) {
                    this.$toast('Favor preencher o email!');
                    return false
                }

                if (this.password !== this.password_confirmation) {
                    this.$toast('As senhas não conferem!');
                    return false
                }

                this.loader = true

                this.$toast('Buscando seus Dados...');

                await this.$axios.post('/resetPassword', {
                    email: this.email,
                    password: this.password,
                    password_confirmation: this.password_confirmation,
                    token: this.token,
                })
                    .then(response => {
                        this.$toast.success(response.data.message);
                        this.$router.push('/')
                    })
                    .catch(error => {
                        this.loader = false
                        this.$toast.error(error.response.data.message);
                    })
            }
        },
        head() {
            return {
                title: 'Recuperação de senha | ' +  `${process.env.PROJECT_NAME}`
            }
        }
    }
</script>

<style scoped>

</style>