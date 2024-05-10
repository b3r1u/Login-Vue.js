<template>
  <div class="boxIn"> 
    <div class="text">
      <h1>{{ currentMode === 'login' ? 'Login' : 'Cadastro' }}</h1>
      <p>{{ currentMode === 'login' ? 'Digite os seus dados de acesso o campo abaixo.' : 'Preencha os campos abaixo para se cadastrar.' }}</p>
    </div>

    <form class="auth-form" @submit.prevent="submitForm">
      <label>E-mail</label>
      <input type="email" v-model="username" name="username" placeholder="Digite seu E-mail" required>
      <br>
      <label>Senha</label>
      <input type="password" v-model="password" name="password" placeholder="Digite sua senha" required>

      <div class="auth-actions">
        <input type="submit" class="submit-btn" :value="currentMode === 'login' ? 'Acessar' : 'Cadastrar'">
        <button type="button" class="cad-form" @click="toggleMode">{{ currentMode === 'login' ? 'Criar Conta' : 'Já tenho uma conta' }}</button>
      </div>

    </form>
  </div>
</template>

<script>

export default {
  name: 'AuthForm',
  data() {
    return {
      username: '',
      password: '',
      currentMode: 'login'
    }
  },
  methods: {
    async submitForm() {
      try {
        if(this.currentMode === 'login'){

        const response = await fetch(`http://localhost:3000/users?user=${this.username}&password=${this.password}`, {
            method: "GET"
        });

          if(response.ok) {
            const data = await response.json();
            if (data.length > 0){
              console.log('Usuário autenticado com sucesso!')
              //Redirecionamento de página
            } else {
              console.log('Erro ao autenticar')
  
            }
          }
        } else {
          
          const data = {
            user: this.username,
            password: this.password,
            nivel: 'usuário'
          }

          const dataJson = JSON.stringify(data);

          const response = await fetch(`http://localhost:3000/users`, {
            method: "POST",
            headers: { 'Content-Type': 'application/json'},
            body: dataJson
          });

          const res = await response.json();
          console.log(`Cadastro feito com sucesso! ${res.id}`)

        }

      } catch (error) {
        console.log('Erro ao processar formulário')
      }
    },
    toggleMode() {
      this.currentMode = this.currentMode === 'login' ? 'register' : 'login';
      this.username = ''
      this.password = ''
    }
  }
}

</script>


<style scoped>

.boxIn {
  background-color: rgb(240, 240, 240);
  width: 400px;
  height: 500px;
  margin: auto;
  margin-top: 10rem;
  border-radius: 5px;
}

p {
  font-size: 14px;
  color: #828181;
}

a {
  font-size: 14px;
  color: #828181;
}

.link {
  text-align: left;
  padding-top: 2rem;
}

label {
  text-align: left;
  margin-bottom: 10px;
  font-weight: bold;
}

.text {
  text-align: left;
  padding-top: 20px;
  margin-left: 2.5rem;
}

.auth-form {
  display: flex;
  flex-direction: column;
  width: 20rem;
  margin: auto;
  padding-top: 25px;
}

input {
  height: 35px;
  border-radius: 4px;
  border: 1px solid #b4b4b4;
  padding: 4px;
  margin-top: 5px;
}

.auth-actions {
  margin-top: 3.5rem;
}

.submit-btn {
  width: 20rem;
  height: 3rem;
  border: none;
  background-color: #f61b93;
  color: #fff;
  font-weight: bold;
  cursor: pointer;
}

.submit-btn:hover {
  background-color: #b91870;
  transition: .5s
}

.cad-form {
  border: none;
  margin-top: 20px;
  color: #f61b93;
  cursor: pointer;
}

</style>
