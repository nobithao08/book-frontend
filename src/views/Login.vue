<template>
    <div class="content p-4 w-75 mx-auto ">
        <Form @submit="login" :validation-schema="userFormSchema" style="width: 800px; margin: 20px auto; background: #D7DDDD;" class="p-3 border rounded border-dark">
        <div class="text-center my-2 fs-3 pb-2 text-dark border-3 fw-bold">ĐĂNG NHẬP</div>
        <div class="row mb-4 mx-2 px-3 pt-3">
            <label for="username" class="col-sm-3 col-form-label fw-bold">Tên đăng nhập:</label>
            <div class="col-sm-9">
                <Field name="username" type="text" class="form-control" v-model="docGia.maDocGia"/>
                <ErrorMessage name="username" class="error-feedback" />
            </div>
        </div>
        <div class="row mb-4 p-3 mx-2">
            <label for="password" class="col-sm-3 col-form-label fw-bold">Mật khẩu:</label>
            <div class="col-sm-9">
                <Field name="password" type="password" class="form-control" v-model="docGia.password"/>
                <ErrorMessage name="password" class="error-feedback" />
            </div>
        </div>
        <p class="ms-4 error-feedback">{{ message }}</p>
        <div class="mx-auto">
            <button type="submit" class="col btn btn-outline-dark ms-4 py-2">Đăng Nhập</button>
            <span class="ms-3">Bạn chưa có tài khoản? </span><router-link :to="{name: 'register'}">Đăng Ký</router-link>
        </div>
    </Form>
    </div>
</template>
<style scoped>
    .error-feedback {
        color: red;
    }
</style>
<script>
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";
import DocGiaService from "@/services/docgia.service";

export default {
    components: {
        Form,
        Field,
        ErrorMessage,
        DocGiaService,
    },
    data() {
        const userFormSchema = yup.object().shape({
            username: yup
                .string()
                .required("Tên đăng nhập không được bỏ trống.")
                .min(4, "Tên đăng nhập phải ít nhất 4 ký tự.")
                .max(30,"Tên đăng nhập không quá 30 kí tự."),
            password: yup
                .string()
                .required("Mật khẩu không được bỏ trống.")
                .min(4, "Mật khẩu phải ít nhất 4 ký tự.")
                .max(20,"Mật khẩu không quá 20 kí tự."),            
        });
        return {
            docGia: {
                maDocGia: "",
                password: "",
            },
            userFormSchema,
            message: '',
        };
    },
    methods:{
        async login(){
            try {
                    const user = await DocGiaService.login(this.docGia);
                    sessionStorage.setItem('userName', user);
                    this.$router.replace({name: "homeBook"});
                } catch (error) {
                    console.log(error);
                    this.message = error.response.data.message;
                }
        },
    },
}
</script>