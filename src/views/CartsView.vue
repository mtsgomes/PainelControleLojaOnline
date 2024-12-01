<template>
  <div class="carts-view">

    <table>
      <thead>
        <tr>
          <th>ID do Carrinho</th>
          <th>Usuário</th>
          <th>Data</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="cart in carts" :key="cart.id">
          <td>{{ cart.id }}</td>
          <td>{{ getUserById(cart.userId) }}</td>
          <td>{{ formatDate(cart.date) }}</td>
          <td>
            <button class="view-button" @click="viewDetails(cart.id)">✚</button>
            <button class="edit-button" @click="openModal(cart.id)">✎</button>
            <button class="delete-button" @click="deleteCart(cart.id)">✖</button>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Modal para Detalhes -->
    <CartModal v-if="showModal" :cart="selectedCart" @close="closeModal" @save="updateCart" />
  </div>
</template>

<script>
import api from "../services/api";
import CartModal from "../components/CartModal.vue";

export default {
  components: {
    CartModal,
  },
  data() {
    return {
      carts: [], // Lista de carrinhos
      users: [], // Dados de usuários para exibir os nomes
      selectedCart: null, // Carrinho selecionado para o modal
      showModal: false, // Controle do modal
    };
  },
  async created() {
    await this.fetchCarts();
    await this.fetchUsers(); // Carrega os usuários
  },
  methods: {
    async fetchCarts() {
      try {
        const response = await api.get("/carts");
        this.carts = response.data;
      } catch (error) {
        console.error("Erro ao carregar carrinhos:", error);
      }
    },
    async fetchUsers() {
      try {
        const response = await api.get("/users");
        this.users = response.data;
      } catch (error) {
        console.error("Erro ao carregar usuários:", error);
      }
    },
    formatDate(date) {
      return new Date(date).toLocaleDateString("pt-BR");
    },
    getUserById(userId) {
      const user = this.users.find((u) => u.id === userId);
      return user ? `${user.name.firstname} ${user.name.lastname}` : "Usuário desconhecido";
    },
    async deleteCart(cartId) {
      try {
        const confirmDelete = confirm("Você tem certeza que deseja excluir este carrinho?");
        if (!confirmDelete) return;

        await api.delete(`/carts/${cartId}`);
        this.carts = this.carts.filter((cart) => cart.id !== cartId);
      } catch (error) {
        console.error("Erro ao excluir carrinho:", error);
      }
    },
    viewDetails(id) {
      this.$router.push(`/carts/${id}`);
    },
    openModal(cart = null) {
      this.selectedCart = cart;
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
      this.selectedCart = null;
    },
    updateCart(updatedCart) {
      const index = this.carts.findIndex((cart) => cart.id === updatedCart.id);
      if (index !== -1) {
        this.carts.splice(index, 1, updatedCart);
      }
      this.closeModal();
    },
    editCart() {
      this.$router.push({
        name: "carts",
        query: { edit: this.cart.id },
      });
    },
    async deleteCart() {
      try {
        const confirmDelete = confirm("Você tem certeza que deseja excluir este carrinho?");
        if (!confirmDelete) return;

        await api.delete(`/carts/${this.cart.id}`);
        alert("carrinho excluído com sucesso!");
        this.goBack();
      } catch (error) {
        console.error("Erro ao excluir carrinho:", error);
        alert("Erro ao excluir o carrinho. Tente novamente.");
      }
    },
  },
};
</script>

<style scoped>

h2 {
  display: flex;
  justify-content: center;
  font-size: 2rem;
  margin-bottom: 20px;
}


table {
  width: 150%;
  border-collapse: collapse;
  margin-top: 25px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

th,
td {
  border: 1px solid #ddd;
  padding: 15px;
  text-align: center;
}

th {
  background-color: #08923d;
  color: #fff;
}

.actions {
  display: flex;
  position: absolute;
}

button {
  padding: 5px 8px;
  border: none;
  background: #08923d;
  color: white;
  border-radius: 10px;
  cursor: pointer;
  display: flex;
}

.view-button {
  background-color: #0068e7;
  color: white;
}

.edit-button {
  background-color: #dda603;
  color: white;
}

.delete-button {
  background-color: #ff0019;
  color: white;
}

.details-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #f9f9f9;
  padding: 10px 15px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 15px;
}


</style>