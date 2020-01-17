<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 sm10 md10 lg8>
      <v-card class="login-card" color="#e8f0ff">
        <v-row no-gutters>

          <v-col class="d-flex justify-content-center d-xs-none d-sm-none d-md-flex" cols="12" md="8">
            <img src="~/assets/img/image.png" width="100%" alt="">
          </v-col>

          <v-col cols="12" md="4">

            <v-card class="p-5" flat height="800px" style="padding: 25px">
              <v-row class="my-5" align="center" justify="center">
                <img src="~/assets/img/logo.png" width="250px" alt="">
              </v-row>
              <h2 class="my-10 py-5 text-center">Acesse sua conta</h2>
              <v-form>
                <v-text-field prepend-icon="mdi-account"
                              @keyup.enter.native="onSubmit"
                              v-model="form.username"
                              name="Username"
                              label="Usuário"
                              placeholder="Insira seu usuário, e-mail ou celular"></v-text-field>

                <v-text-field prepend-icon="mdi-lock"
                              @keyup.enter.native="onSubmit"
                              v-model="form.password"
                              name="Password"
                              label="Senha"
                              type="password"
                              placeholder="Insira sua senha"></v-text-field>

                <p class="caption text-right mt-n4">
                  <nuxt-link to="/auth/reset">Esqueceu sua senha?</nuxt-link>
                </p>
                <v-card-actions>
                  <v-btn class="py-7" primary rounded dark large block color="indigo">Entrar</v-btn>
                </v-card-actions>
                <footer class="mt-12 pt-12 text-center">
                  <p>
                    Não possui uma conta?
                    <nuxt-link to="/auth/register">CADASTRE-SE</nuxt-link>
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
    data() {
      return {
        error: null,
        loader: true,
        form: {
          username: null,
          password: null,
        },
        confirm: null
      }
    },
    async mounted() {
      this.confirm = this.$route.query.confirm
      this.loader = false
    },
    methods: {
      async onSubmit() {
        if (!this.isFilled) {
          this.$toast('Favor preencher os dados de acesso')
          return false
        }

        this.error = false
        this.loader = true

        this.$toast('Verificando seus Dados!');

        await this.$auth.loginWith('local', {
          data: {
            username: this.form.username,
            password: this.form.password,
          }
        }).then(response => {
          this.$toast.success('Dados verificados com suceso!');
          this.$router.push('/')
        }).catch(error => {
          this.loader = false
          this.error = error.response.data.message
          this.resetForm();
        })
      },
      resetForm() {
        this.form.username = null;
        this.form.password= null;
      }
    },
    computed: {
      ifFilled() {
        return this.form.username && this.form.password;
      }
    },
    components: {},
    head() {
      return {
        title: "Entrar | " + `${process.env.PROJECT_NAME}`
      }
    }

  }
</script>
<style>
  .login-card {
    position: relative;
    width: 1300px;
    height: 800px;
    box-shadow: 1px 1px 50px rgba(58, 72, 129, 0.52);
  }
  v-card-title {
    text-align: center !important;
  }
</style>
