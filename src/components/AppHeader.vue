<template>
    <nav class="navbar navbar-dark bg-dark p-2 row">
        <div class="col-md-4 d-flex">
            <a href="/" class="navbar-brand fw-bold">CỬA HÀNG SÁCH</a>
        </div>
        <div class="col-md">
            <ul v-if="!getUserName" class="nav d-flex justify-content-end">
                <li class="nav-item">
                    <router-link :to="{ name: 'register' }" class="nav-link text-white">
                        ĐĂNG KÝ
                    </router-link>
                </li>
                <li class="nav-item">
                    <router-link :to="{ name: 'login' }" class="nav-link text-white">
                        ĐĂNG NHẬP
                    </router-link>
                </li>
            </ul>
            <ul v-else class="nav d-flex justify-content-end">
                <li class="nav-item">
                    <router-link :to="{ name: 'rentList' }" class="nav-link text-light">
                        LỊCH SỬ MƯỢN
                    </router-link>
                </li>
                <li class="nav-item">
                    <router-link :to="{ name: 'docgia-info', params: { maDocGia: getUserName } }" class="nav-link text-white">
                        TÀI KHOẢN
                    </router-link>
                </li>
                <li class="nav-item">
                    <a href="#" class="nav-link text-white" @click="logout">ĐĂNG XUẤT</a>
                </li>
            </ul>
        </div>
    </nav>
</template>

<script>
export default {
    data() {
        return {
            userName: '',
        }
    },
    computed: {
        getUserName() {
            this.userName = sessionStorage.getItem('userName');
            return this.userName;
        }
    },
    methods: {
        logout() {
            if (confirm('Bạn có chắc muốn đăng xuất không?')) {
                sessionStorage.removeItem("userName");
                this.userName = "";
                window.location.reload();
            }
        }
    },
    mounted() {
        this.getUserName;
        if (!this.userName) {
            this.$router.push({ name: "login" });
        }
    }
};
</script>