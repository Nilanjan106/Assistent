<script>
import { onMounted, ref, watch } from 'vue'
import { useRouter } from 'vue-router'
import Navbar from './components/Navbar.vue'
import Navbar1 from './components/Navbar1.vue'

export default {
  name: 'App',
  components: {
    Navbar,
    Navbar1
  },
  setup() {
    const isLoggedIn = ref(false);
    const router = useRouter();

    // Check authentication status on mount
    onMounted(() => {
      checkAuthStatus();
    });

    // Watch for route changes to check auth status
    watch(() => router.currentRoute.value, () => {
      checkAuthStatus();
    });

    const checkAuthStatus = () => {
      const token = localStorage.getItem('token');
      isLoggedIn.value = !!token;
    };

    return {
      isLoggedIn
    }
  }
}
</script>

<template>
  <div id="app">
    <!-- Render Navbar1 for authenticated users, Navbar for non-authenticated users -->
    <Navbar1 v-if="isLoggedIn" />
    <Navbar v-else />
    <router-view></router-view>
  </div>
</template>

<style>
#app {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  min-height: 100vh;
  background-color: #f5f7fa;
}

/* Global styles */
body {
  margin: 0;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Fira Sans', 'Droid Sans', 'Helvetica Neue', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: #f5f7fa;
}

#app {
  min-height: 100vh;
}

/* FUTUREWISE specific styles */
.futurewise-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.futurewise-card {
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
  margin-bottom: 1.5rem;
}

.futurewise-title {
  color: #2c3e50;
  font-size: 1.8rem;
  font-weight: 600;
  margin-bottom: 1rem;
}

.futurewise-subtitle {
  color: #34495e;
  font-size: 1.2rem;
  font-weight: 500;
  margin-bottom: 0.5rem;
}
</style>
