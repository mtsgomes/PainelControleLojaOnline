<template>
    <div class="login-view">
        <form @submit.prevent="handleLogin">
            <h2>Login</h2>
            <label for="username">Usuário</label>
            <input type="text" id="username" v-model="form.username" placeholder="Digite seu usuário" required
                :class="{ 'input-error': error }" aria-label="Usuário" />

            <label for="password">Senha</label>
            <input type="password" id="password" v-model="form.password" placeholder="Digite sua senha" required
                :class="{ 'input-error': error }" aria-label="Senha" />

            <button type="submit">Entrar</button>

            <p v-if="error" class="error">{{ error }}</p>
        </form>
    </div>
</template>

<script>
import api from "../services/api";

export default {
    data() {
        return {
            form: {
                username: "",
                password: "",
            },
            error: null,
        };
    },
    methods: {
        async handleLogin() {
            const fixedUsername = "usuario123"; // Usuário fixo para login rápido
            const fixedPassword = "senha123"; // Senha fixa para login rápido

            if (!this.form.username || !this.form.password) {
                this.error = "Todos os campos são obrigatórios.";
                return;
            }

            if (this.form.username === fixedUsername && this.form.password === fixedPassword) {
                // Login rápido com usuário e senha fixos
                localStorage.setItem("authToken", "fakeToken123");
                this.$router.push("/");
                return;
            }

            try {
                const response = await api.post("/auth/login", this.form);
                const token = response.data.token;

                if (token) {
                    localStorage.setItem("authToken", token);
                    this.$router.push("/");
                } else {
                    this.error = "Token não recebido. Tente novamente.";
                }
            } catch (err) {
                this.error = "Credenciais inválidas. Tente novamente.";
            }
        },
    },
};
</script>

<style scoped>
.login-view {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 80vh;
    background-color: #1e1e1e; 
    font-family: 'Arial', sans-serif;
}


form {
    background: #2e2e2e; 
    padding: 30px;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    width: 350px;
    display: flex;
    flex-direction: column;
    transition: all 0.3s ease;
}

form:hover {
    box-shadow: 0 6px 8px rgba(0, 0, 0, 0.15);
}


h2 {
    margin-bottom: 20px;
    text-align: center;
    color: #08923d;; 
    font-size: 1.5rem;
    font-weight: bold;
}


label {
    margin-bottom: 5px;
    font-weight: bold;
    color: #ddd; 
}


input {
    margin-bottom: 15px;
    padding: 10px;
    border: 1px solid #444; 
    border-radius: 5px;
    font-size: 14px;
    background-color: #3e3e3e; 
    color: #fff; 
    transition: border-color 0.3s ease;
}

input:focus {
    border-color: #08923d;; /* Verde */
    outline: none;
}

input.input-error {
    border-color: red;
}

button {
    background-color: #08923d;; 
    color: white;
    border: none;
    padding: 10px 15px;
    border-radius: 5px;
    cursor: pointer;
    font-weight: bold;
    transition: background-color 0.3s ease;
}

button:hover {
    background-color: #388e3c;
}

.error {
    color: red;
    font-size: 14px;
    margin-top: 10px;
    text-align: center;
}

h2 {
    font-size: 1.3rem;
}

button {
    font-size: 14px;
}
</style>
