<template>
    <div class="register-container">
        <div class="left-column">
            <h2>註冊</h2>
            <form @submit.prevent="register">
                <div class="form-control">
                    <label for="username">用戶名</label>
                    <input v-model="username" type="text" id="username" placeholder="輸入用戶名">
                </div>
                <div class="form-control">
                    <label for="email">電子郵件</label>
                    <input v-model="email" type="email" id="email" placeholder="輸入電子郵件">
                </div>
                <div class="form-control">
                    <label for="password">密碼</label>
                    <input v-model="password" type="password" id="password" placeholder="輸入密碼">
                </div>
                <button type="submit" class="btn">註冊</button>
            </form>
        </div>
        <div class="right-column">
            qweqe
            <!-- <img src="path/to/your/image.jpg" alt="註冊圖片"> -->
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const username = ref('');
const email = ref('');
const password = ref('');

const register = async () => {
    const data = {
        username: username.value,
        email: email.value,
        password: password.value,
    };
    try {
        const response = await fetch('http://127.0.0.1:5000/register', {
            method: 'POST',
            body: JSON.stringify(data),
            headers: { 'Content-Type': 'application/json' },
        });
        if (response.ok) {
            alert('註冊成功');
        } else {
            alert('註冊失敗');
        }
    } catch (error) {
        console.error('Error:', error);
    }
};
</script>

<style scoped>
.register-container {
    display: flex;
    justify-content: space-between;
}

.left-column {
    flex: 1;
    padding: 20px;
}

.right-column {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
}

.right-column img {
    max-width: 100%;
    height: auto;
}
</style>