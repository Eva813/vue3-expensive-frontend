<template>
    <div class="register-container">
        <div class="left-column">
            <h2>註冊</h2>
            <button type="button" class="google-login-btn" @click="handleGoogleLogin">
                <div class="mx-2 flex items-center pl-3">
                    <svg class="mx-auto h-5 w-5" viewBox="0 0 24 24" width="24" height="24"
                        xmlns="http://www.w3.org/2000/svg">
                        <g transform="matrix(1, 0, 0, 1, 27.009001, -39.238998)">
                            <path fill="#4285F4"
                                d="M -3.264 51.509 C -3.264 50.719 -3.334 49.969 -3.454 49.239 L -14.754 49.239 L -14.754 53.749 L -8.284 53.749 C -8.574 55.229 -9.424 56.479 -10.684 57.329 L -10.684 60.329 L -6.824 60.329 C -4.564 58.239 -3.264 55.159 -3.264 51.509 Z" />
                            <path fill="#34A853"
                                d="M -14.754 63.239 C -11.514 63.239 -8.804 62.159 -6.824 60.329 L -10.684 57.329 C -11.764 58.049 -13.134 58.489 -14.754 58.489 C -17.884 58.489 -20.534 56.379 -21.484 53.529 L -25.464 53.529 L -25.464 56.619 C -23.494 60.539 -19.444 63.239 -14.754 63.239 Z" />
                            <path fill="#FBBC05"
                                d="M -21.484 53.529 C -21.734 52.809 -21.864 52.039 -21.864 51.239 C -21.864 50.439 -21.724 49.669 -21.484 48.949 L -21.484 45.859 L -25.464 45.859 C -26.284 47.479 -26.754 49.299 -26.754 51.239 C -26.754 53.179 -26.284 54.999 -25.464 56.619 L -21.484 53.529 Z" />
                            <path fill="#EA4335"
                                d="M -14.754 43.989 C -12.984 43.989 -11.404 44.599 -10.154 45.789 L -6.734 42.369 C -8.804 40.429 -11.514 39.239 -14.754 39.239 C -19.444 39.239 -23.494 41.939 -25.464 45.859 L -21.484 48.949 C -20.534 46.099 -17.884 43.989 -14.754 43.989 Z" />
                        </g>
                    </svg>
                </div>
                <span class="text-slate-500 group-hover:text-slate-600">Sing in with Google</span>
            </button>
            <!-- <p>
                {{ data }}
            </p> -->

            <form @submit.prevent="handleRegister">
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
import { googleTokenLogin } from 'vue3-google-login'

const username = ref('');
const email = ref('');
const password = ref('');



const GOOGLE_CLIENT_ID = import.meta.env.VITE_GOOGLE_CLIENT_ID;

const handleGoogleLogin = () => {
    googleTokenLogin({
        clientId: GOOGLE_CLIENT_ID
    }).then((response) => {
        console.log(response)
    })
}

// const addTransaction = async () => {
//   const data = {
//     name: name.value,
//     price: parseFloat(price.value),
//     type: parseFloat(price.value) > 0 ? 'income' : 'outcome',
//   };

//   item_list.value.push(data);
//   name.value = '';
//   price.value = '';

//   try {
//     const response = await fetch('http://127.0.0.1:5000/item/', {
//       method: 'POST',
//       body: JSON.stringify(data),
//       headers: { 'Content-Type': 'application/json' },
//     });
//     const result = await response.json();
//     console.log(result);
//   } catch (error) {
//     console.error('Error:', error);
//   }
// };
const handleRegister = async () => {
    const data = {
        username: username.value,
        email: email.value,
        password: password.value,
    };
    try {
        const response = await fetch('http://127.0.0.1:5000/register/', {
            method: 'POST',
            body: JSON.stringify(data),
            headers: { 'Content-Type': 'application/json' },
        });
        if (response.ok) {
            alert('註冊成功');
        } else {
            console.log(response)
            alert(response);
        }
    } catch (error) {
        console.error('Error:', error);
    }
};
// const callback = (response) => {
//     data.value = response
//     console.log(response)
// }
</script>

<style>
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

.google-login-btn {
    background-color: #ffffff;
    /* 白色背景 */
    border: 1px solid #dadce0;
    /* 添加邊框 */
    border-radius: 4px;
    /* 輕微圓角 */
    display: flex;
    align-items: center;
    padding: 8px 16px;
    /* 增加內邊距 */
    cursor: pointer;
    transition: background-color 0.2s ease, box-shadow 0.2s ease;
    box-shadow: 0px 1px 2px rgba(0, 0, 0, 0.05);
}

.google-login-btn:hover {
    background-color: #f8f9fa;
}

.google-login-btn svg {
    margin-right: 8px;
    /* 圖標與文字之間的距離 */
}
</style>