<template>
    <div>
        <NavBar />
        <div class="container">
            <h1>My Laptops</h1>
            <div class="row">
                <div class="col-6">
                    <h5>List of Laptops</h5>
                    <hr>
                    <ul class="list-group">
                        <li class="list-group-item btn-li"  v-for="laptop in laptops" :key="laptop.id" @click="onSelected(laptop)">
                            {{laptop.brand}} <span class="float-right">{{laptop.value}}</span>
                        </li>
                    </ul>
                </div>
                <div class="col-4">
                    <LaptopView :laptop="selectedLaptop" @saved="onChange" @newLaptop="onNew" @deleted="onChange" />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            laptops: [],
            selectedLaptop: {}
        }
    },
    methods: {
        async getMyLaptops() {
            await this.$axios.get('/api/laptops')
            .then((res)=>{
                if(res.status==200) {
                    this.laptops = res.data
                }
            })
        },
        onChange() {
            this.getMyLaptops()
            this.selectedLaptop = {}
        },
        onSelected(laptop) {
            this.selectedLaptop = laptop;
        },
        onNew() {
            this.selectedLaptop = {}
        },
    },
    created() {
        this.getMyLaptops()
    }
}
</script>

<style>
.row {
    padding-top: 30px;
}
.container {
    margin-top: 20px;
}
h1 {
    text-align: center;
}
.btn-li {
    cursor: pointer;
}
.btn-li:hover {
    background-color: rgb(25, 48, 14);
}
</style>
