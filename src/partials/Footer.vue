<script setup>
const currentYear = new Date().getFullYear()
</script>

<template>
    <footer class="footer mt-5 py-4 bg-light">
        <div class="container">
            <div class="row align-items-center">
                <div class="col-md-6 text-center text-md-start">
                    <p class="mb-0 text-muted">
                        &copy; {{ currentYear }} Portfolio. All rights reserved.
                    </p>
                </div>
                <div class="col-md-6 text-center text-md-end mt-3 mt-md-0">
                    <div class="social-links">
                        <a href="https://github.com" target="_blank" class="text-muted me-3">
                            <i class="bi bi-github"></i>
                        </a>
                        <a href="https://linkedin.com" target="_blank" class="text-muted me-3">
                            <i class="bi bi-linkedin"></i>
                        </a>
                        <a href="mailto:example@email.com" class="text-muted">
                            <i class="bi bi-envelope"></i>
                        </a>
                    </div>
                </div>
            </div>
        </div>
    </footer>
</template>

<style scoped>
.footer {
    border-top: 1px solid rgba(0, 0, 0, 0.1);
}

.social-links a {
    text-decoration: none;
    font-size: 1.2rem;
    transition: color 0.3s ease;
}

.social-links a:hover {
    color: #42b883 !important;
}
</style>
