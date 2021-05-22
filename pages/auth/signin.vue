<template>
  <v-row align="center" justify="center">
    <v-col cols="12" sm="8" md="4" align="center">
      <v-card width="500" class="elevation-4 text-left" shaped color="yellow">
        <v-card-title>Login</v-card-title>
        <v-card-subtitle>Login de Acesso</v-card-subtitle>
        <v-card-text>
          <v-form>
            <v-text-field
              label="E-mail"
              name="login"
              prepend-icon="mdi-account"
              type="text"
              v-model="auth.email"
            ></v-text-field>

            <v-text-field
              label="Senha"
              name="password"
              prepend-icon="mdi-lock"
              type="password"
              v-model="auth.password"
            ></v-text-field>
          </v-form>
        </v-card-text>
        <v-card-actions class="text-center">
          <v-btn
            class="login-button"
            @click="login"
            depressed
            large
            >Entrar</v-btn
          >
          <v-btn class="reset-button" @click="forgotPassword" depressed large
            >Esqueci minha senha</v-btn
          >
        </v-card-actions>
      </v-card>
      <v-snackbar
        :timeout="4000"
        v-model="snackbar"
        absolute
        bottom
        center
      >
        {{ snackbarText }}
      </v-snackbar>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      snackbar: false,
      snackbarText: 'Sem erro de mensagem',
      auth: {
        email: '',
        password: ''
      }
    }
  },
  methods: {
    login() {
      let that = this
      this.$fire.auth.signInWithEmailAndPassword(this.auth.email, this.auth.password)
      .catch(function (error){
        that.snackbarText = error.message
        that.snackbar = true
      }).then((user) => {
        //we are signed in
        $nuxt.$router.push('/')
      })
    },
    forgotPassword() {
      let that = this
      this.$fire.auth.sendPasswordResetEmail(this.auth.email)
      .then(function (){
        that.snackbarText = 'Processo de recuperação de senha enviado para o email:' + that.auth.email
        that.snackbar = true
      })
      .catch(function (error) {
        that.snackbarText = error.message
        that.snackbar = true
      })
    }
  }
}
</script>

<style>

</style>