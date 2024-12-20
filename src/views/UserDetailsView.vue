<template>
  <div class="user-details-view">

    <div v-if="user" class="profile-container">
      <div class="profile-header">
        <div class="avatar">
          <img :src="user.image || 'https://via.placeholder.com/150'" alt="User Avatar" />
        </div>
        <div class="user-info">
          <h2>{{ user.name.firstname }} {{ user.name.lastname }}</h2>
          <p class="email">{{ user.email }}</p>
        </div>
      </div>

      <div class="profile-details">
        <div class="detail-row">
          <p><strong>Telefone:</strong> {{ user.phone }}</p>
        </div>
        <div class="detail-row">
          <p><strong>Endereço:</strong> {{ user.address.street }}, {{ user.address.city }}</p>
        </div>
        <div class="detail-row">
          <p><strong>CEP:</strong> {{ user.address.zipcode }}</p>
        </div>
      </div>

      <div class="actions">
        <button @click="editUser" class="edit-button">Editar Perfil</button>
        <button @click="deleteUser" class="delete-button">Excluir Perfil</button>
        <button @click="goBack" class="back-button">Voltar</button>
      </div>
    </div>

    <p v-else class="loading">Carregando os detalhes do usuário...</p>

    <UserModal
      v-if="showModal"
      :user="user"
      @close="closeModal"
      @save="saveUser"
    />

  </div>
</template>

<script>
import UserModal from "@/components/UserModal.vue";
import api from "../services/api";

export default {
  data() {
    return {
      user: null, // Dados do usuário
    };
  },
  async created() {
    await this.fetchUserDetails();
  },
  methods: {
    async fetchUserDetails() {
      try {
        const { id } = this.$route.params; // Obtém o ID do usuário via parâmetros da rota
        const response = await api.get(`/users/${id}`);
        this.user = response.data;
      } catch (error) {
        console.error("Erro ao carregar detalhes do usuário:", error);
        this.$router.push("/users"); // Redireciona em caso de erro
      }
    },
    goBack() {
      this.$router.push("/users"); // Retorna à lista de usuários
    },
    editUser() {
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
    },
    async deleteUser() {
      try {
        const confirmDelete = window.confirm("Tem certeza que deseja excluir este usuário?");
        if (confirmDelete) {
          await api.delete(`/users/${this.user.id}`);
          this.$router.push("/users"); // Redireciona para a lista de usuários após exclusão
        }
      } catch (error) {
        console.error("Erro ao excluir o usuário:", error);
      }
    },
  },
};
</script>

<style scoped>
.user-details-view {
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #42454780;
  height: 500px;
  width: 200%;
  border-radius: 5%;
}

.profile-container {
  max-width: 900px;
  height: 400px;
  background-color: #42454780;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  padding: 30px;
  text-align: center;
}

.profile-header {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 30px;
}

.avatar {
  margin-right: 20px;
}

.avatar img {
  width: 150px;
  height: 150px;
  border-radius: 50%;
  object-fit: cover;
}

.user-info h2 {
  font-size: 1.8rem;
  color: white;
}

.user-info .email {
  font-size: 1.1rem;
  color: #555;
  margin-top: 5px;
  color: white;
}

.profile-details {
  margin-top: 30px;
  text-align: left;
}

.detail-row {
  margin-bottom: 20px;
  font-size: 1.2rem;
  color: white;
}

.actions {
  margin-top: 70px;
  display: flex;
}


button {
  padding: 10px 15px;
  font-size: 1rem;
  font-weight: bold;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.loading {
  font-size: 1.2rem;
  color: white;
  text-align: center;
}
</style>