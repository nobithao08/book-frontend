<template>
    <div v-if="rentBooks.length == 0" class="text-center fs-3  text-dark p-5 content ">
        Không tìm thấy lịch sử mượn sách 
    </div>
    <div v-else class="mx-auto content  w-75">
        <table class="table table-hover ">
        <thead>
            <tr>
                <th>STT</th>
                <th>Tên Sách</th>
                <th>Ngày mượn</th>
                <th>Ngày trả</th>
                <th>Trạng thái</th>
            </tr>
        </thead>
        <tbody>
            <RentCard v-for="(book,index) in rentBooks" :key="book.maDocGia" :rentBook="book" :type="type" :index="index"/>
        </tbody>
    </table>
    </div>
</template>
<script>
import theodoimuonsachService from '@/services/theodoimuonsach.service';
import RentCard from '@/components/RentCard.vue';
export default {
    data() {
        return {
            rentBooks:[],
            type:-1,
        }
    },
    components:{
        RentCard,
    },
    methods: {
        async getRentBooks(){
            this.rentBooks = await theodoimuonsachService.get(sessionStorage.getItem("userName"));
        },
        showType(type){
            this.type = type;
        }
    },
    mounted(){
        this.getRentBooks();
    }
}
</script>