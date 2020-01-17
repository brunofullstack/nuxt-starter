<template>
    <v-layout column justify-center align-center>
        <v-flex xs12 sm10 md10 lg8>
            <v-card class="p-5" flat style="padding: 25px">
                <v-row class="my-5" align="center" justify="center">
                    <img src="~/assets/img/logo.png" width="60%" alt="">
                </v-row>
                <h2 class="my-10 py-5 text-center">Recuperação de senha</h2>
                <v-form>
                    <v-text-field prepend-icon="mdi-envelope-o"
                                  @keyup.enter.native="onSubmit"
                                  v-model="email"
                                  name="e-mail"
                                  label="E-mail"
                                  type="email"
                                  placeholder="Insira seu e-mail"></v-text-field>
                    <v-card-actions>
                        <v-btn class="py-7" primary rounded dark large block color="indigo">Recuperar Senha</v-btn>
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
            }
        },
        async mounted() {
            this.loader = false
        },
        methods: {
            loginRoute() {
                this.$router.push('/')
            },
            async onSubmit() {
                if (!this.email) {
                    this.$toast('Favor preencher o email');
                    return false
                }

                this.loader = true

                this.$toast('Buscando seus dados...')

                await this.$axios.post('/sendPasswordResetLink',
                    {email: this.email}
                ).then(response => {
                    this.$toast.success(response.data.message)
                    this.$router.push('/')
                }).catch(error => {
                    this.loader = false
                    this.$toast.error(error.response.data.message)
                    this.email = null
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