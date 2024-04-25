<template>
    <div class="col-md-5 mt-2">
            <SearchBook v-model="searchText" />
    </div>
    <div class="row">
        <div class="row w-70 mx-20" style="margin-top: 20px;">
        <div class="col-4 p-0 me-1">
            <h3 class="text-dark">DANH SÁCH SÁCH</h3>
            <BookList :books="books" :bookIndex="bookIndex" @show:book="getShowBook"/>
        </div>
        <div class="col rounded p-2 mx-5" v-if="bookIndex >= 0" style="background: #CFE5E4;">
            <BookCard :book="filteredBooks" />
            <div v-if="filteredBooks.soQuyen > 0">
                <button type="button" class="btn btn-outline-danger" @click="showRent">Mượn sách</button>
                <form @submit.prevent="RentBook(filteredBooks)" style="width: 60%;"
                    class="border rounded border-2 p-2 border-dark mt-2 mx-auto" v-if="showRentBook">
                    <label class="form-lable">Ngày mượn</label>
                    <input v-model="rentBook.ngayMuon" type="date" class="form-control" required disabled />
                    <label class="form-lable">Ngày trả</label>
                    <input v-model="rentBook.ngayTra" :min="rentBook.ngayMuon" type="date" required class="form-control" />
                    <button class="btn btn-outline-primary mt-2">Xác nhận mượn</button>
                </form>
            </div>
            <div v-else>
                Đã hết
            </div>
        </div>
    </div>
    </div>
   
</template>
<script>
import BookService from '@/services/book.service';
import BookList from '@/components/BookList.vue';
import BookCard from '@/components/BookCard.vue';
import SearchBook from "@/components/InputSearch.vue";
import theodoimuonsachService from '@/services/theodoimuonsach.service';


export default {
    data() {
        return {
            books: [],
            bookIndex: -1,
            searchText: "",
            rentBook: {
                maDocGia: "",
                maSach: "",
                ngayMuon: new Date().toISOString().substr(0, 10),
                ngayTra: "",
                trangThai:0
            },
            showRentBook:false,
        }
    },
    components: {
        BookList,
        BookCard,
        SearchBook,
    },
    computed: {
        filteredBooks() {
            return this.books[this.bookIndex];
        },
        bookStrings() {
            return this.books.map((book) => {
                const { tenSach } = book;
                return [tenSach].join();
            });
        },
        filteredSearch() {
            if (!this.searchText) {
                return this.books;
            }
            return this.books.filter((_book, index) => {
                return this.bookStrings[index].includes(this.searchText)
            });

        },
        activeSearch() {
            if (this.activeIndex < 0) {
                return null;
            }
            else {
                return this.filteredSearch[this.activeIndex];
            }

        },
        filteredSearchsCount() {
            return this.filteredSearchs.length;
        },
    },
    methods: {
        showRent(){
            this.showRentBook = !this.showRentBook;
        },
        async getBook() {
            try {
                this.books = await BookService.getAll();
            }
            catch (error) {
                console.log('There was an error!', error);
            }
        },
        async retrieveBooks() {
            try {
                this.books = await BookService.getAll();
            } catch (error) {
                console.log(error);
            }
        },
        getShowBook(bookIndex) {
            this.bookIndex = bookIndex;
        },
        async RentBook(book) {
            this.rentBook.maDocGia = sessionStorage.getItem("userName");
            this.rentBook.maSach = book._id;
            console.log(book);
            try{
                const result = await theodoimuonsachService.create(this.rentBook);
                book.soQuyen =book.soQuyen - 1;
                await BookService.update(book._id,book);
                alert("Đăng kí mượn sách thành công!");
            }
            catch(error){
                console.log(error);
            }
        }
    },
    mounted() {
        if (localStorage.getItem('reloaded')) {
            localStorage.removeItem('reloaded');
        } else {
            localStorage.setItem('reloaded', '1');
            location.reload();
        }
        this.getBook();
    }
}

</script>

