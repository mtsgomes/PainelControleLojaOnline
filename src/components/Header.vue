<script setup>
import { RouterLink} from 'vue-router';

</script>

<template>
    <header>
        <h1>Fake Store</h1>
        <div class="wrapper">
            <nav>
                <RouterLink to="/" exact>Home</RouterLink>
                <RouterLink to="/products">Produtos</RouterLink>
                <RouterLink to="/carts">Carrinhos</RouterLink>
                <RouterLink to="/users">Usuários</RouterLink>
            </nav>
        </div>
    </header>
</template>

<style>
header {
    background-color: #42454780;
    padding: 5px 0; 
    position: fixed;
    width: 100%;
    top: 0;
    left: 0;
    z-index: 1000;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.wrapper {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
}

nav {
    display: flex;
    justify-content: space-around;
}

RouterLink:hover {
    text-decoration: underline;
}

h1 {
    padding-left: 20px;
    margin: 0; 
    font-size: 1.5em; 
}

main {
    padding-top: 50px; 
}
</style>
