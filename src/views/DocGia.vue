<script>
import DocgiaService from "@/services/docgia.service.js";
export default {
    props: {
        maDocGia: { type: String, required: true },
    },
    data() {
        return {
            user: [],
        }
    },
    methods: {
        async getUser() {
            try {
                this.user = await DocgiaService.get(this.maDocGia);
                console.log(this.user);
            }
            catch (error) {
                console.log(error);
            }
        }
    },
    created() {
        this.getUser();
    }
}
</script>

    <template>
    <div style="margin: 3px auto auto auto; width: 50%;" class="content p-2">
        <table class="table table-hover  p-2">
            <thead>
                <tr>
                    <th scope="col" colspan="2" class="text-center text-dark">TÀI KHOẢN</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <th scope="row">Tên đăng nhập:</th>
                    <td>{{ user._id }}</td>
                </tr>
                <tr>
                    <th scope="row">Họ:</th>
                    <td>{{ user.hoLot }}</td>
                </tr>
                <tr>
                    <th scope="row">Tên:</th>
                    <td>{{ user.ten }}</td>
                </tr>
                <tr>
                    <th scope="row">Giới tính:</th>
                    <td>{{ user.phai }}</td>
                </tr>
                <tr>
                    <th scope="row">Địa Chỉ:</th>
                    <td>{{ user.diaChi }}</td>
                </tr>
                <tr>
                    <th scope="row">Số điện thoại:</th>
                    <td>{{ user.dienThoai }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
<style scoped>
.error-feedback {
    color: red;
}
</style>
