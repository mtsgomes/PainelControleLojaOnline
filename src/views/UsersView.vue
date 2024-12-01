<template>
    <div class="users-view">
        <div class="controls">
            <input v-model="searchQuery" @input="filterUsers" placeholder="Pesquisar por nome ou e-mail" />
            <button @click="openModal()">Adicionar Usuário</button>
        </div>

        <table>
            <thead>
                <tr>
                    <th>Nome</th>
                    <th>E-mail</th>
                    <th>Cidade</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="user in filteredUsers" :key="user.id">
                    <td>{{ user.name.firstname }} {{ user.name.lastname }}</td>
                    <td>{{ user.email }}</td>
                    <td>{{ user.address.city }}</td>
                    <td>
                        <button class="view-button" @click="viewDetails(user.id)">✚</button>
                        <button class="edit-button" @click="openModal(user)">✎</button>
                        <button class="delete-button" @click="deleteUser(user.id)">✖</button>
                    </td>
                </tr>
            </tbody>
        </table>

        <UserModal v-if="showModal" :user="selectedUser" @close="closeModal" @save="saveUser" />
    </div>
</template>
<script>
import api from "../services/api";
import UserModal from "../components/UserModal.vue";

export default {
    data() {
        return {
            users: [],
            filteredUsers: [],
            searchQuery: "",
            showModal: false,
            selectedUser: null,
        };
    },
    async created() {
        await this.fetchUsers();
    },
    methods: {
        async fetchUsers() {
            try {
                const response = await api.get("/users");
                this.users = response.data;
                this.filteredUsers = [...this.users];
            } catch (error) {
                console.error("Erro ao buscar usuários:", error);
            }
        },
        filterUsers() {
            const query = this.searchQuery.toLowerCase();
            this.filteredUsers = this.users.filter(
                (user) =>
                    user.name.firstname.toLowerCase().includes(query) ||
                    user.name.lastname.toLowerCase().includes(query) ||
                    user.email.toLowerCase().includes(query)
            );
        },
        openModal(user = null) {
            this.selectedUser = user;
            this.showModal = true;
        },
        closeModal() {
            this.showModal = false;
        },
        async saveUser(user) {
            try {
                if (user.id) {
                    await api.put(`/users/${user.id}`, user);
                } else {
                    const response = await api.post("/users", user);
                    this.users.push(response.data);
                }
                this.closeModal();
                await this.fetchUsers();
            } catch (error) {
                console.error("Erro ao salvar usuário:", error);
            }
        },
        async deleteUser(id) {
            try {
                const confirmDelete = confirm(
                    "Você tem certeza que deseja excluir este usuário?"
                );
                if (!confirmDelete) return;

                await api.delete(`/users/${id}`);
                this.users = this.users.filter((user) => user.id !== id);
                this.filteredUsers = [...this.users];
            } catch (error) {
                console.error("Erro ao excluir usuário:", error);
            }
        },
        viewDetails(id) {
            this.$router.push(`/users/${id}`);
        },
    },
    components: {
        UserModal,
    },
};
</script>

<style scoped>
.controls input {
    width: 100%;
    max-width: 400px;
    padding: 10px 15px;
    font-size: 1rem;
    border: 1px solid #ddd;
    border-radius: 5px;
    background-color: #f9f9f9;
    color: #333;
    transition: all 0.3s ease;
    box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
}

.controls input:focus {
    outline: none;
    border-color: #007bff;
    background-color: #ffffff;
    box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
}

.controls input::placeholder {
    color: #aaa;
    font-style: italic;
}

table {
    width: 200%;
}

td {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: center;
  table-layout: fixed;
}

</style>