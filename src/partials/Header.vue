<script setup>
import { ref } from 'vue'

const isNavOpen = ref(false)

const toggleNav = () => {
    isNavOpen.value = !isNavOpen.value
}
</script>

<template>
    <header class="header py-3 bg-white shadow-sm">
        <div class="container">
            <nav class="navbar navbar-expand-lg navbar-light">
                <a class="navbar-brand fw-bold text-primary" href="#">Portfolio</a>

                <button class="navbar-toggler" type="button" @click="toggleNav">
                    <span class="navbar-toggler-icon"></span>
                </button>

                <div class="collapse navbar-collapse" :class="{ 'show': isNavOpen }">
                    <ul class="navbar-nav ms-auto">
                        <li class="nav-item">
                            <a class="nav-link" href="#">Home</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#projects">Projects</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#about">About</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#contact">Contact</a>
                        </li>
                    </ul>
                </div>
            </nav>
        </div>
    </header>
</template>

<style scoped>
.header {
    position: sticky;
    top: 0;
    z-index: 1000;
    background-color: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(5px);
}

.navbar-brand {
    font-size: 1.5rem;
}

.nav-link {
    font-weight: 500;
    color: #666;
    transition: color 0.3s ease;
}

.nav-link:hover {
    color: #42b883;
}
</style>
