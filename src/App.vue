<script>
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';

export default {
    name: "App",
    components: {
        Header,
        Footer,
    },
};
</script>

<template>
    <div>
        <div>
            <Header />
        </div>
        
        <main>
            <RouterView />
        </main>

        <Footer />
        
    </div>
</template>

<style>
main {
    padding-top: 90px; 
    padding-bottom: 60px;
}

</style>
