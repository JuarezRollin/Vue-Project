<template>
    <div class="content text-black flex justify-center items-center" style="height: 85vh;">
        <div class="alert alert-success" role="alert" v-show="ShowSuccessModal" style="position: absolute; left: 50%;top:3%; transform: translate(-50%, 0);z-index: 3;">
            {{ modified ? "Successfully Modified!" : (isDeleted ? "Successfully Deleted!" : "Successfully Created!") }}
        </div>

        <data class="dimBg" v-show="viewModal" @click="viewModal=false"></data>
        <div class="modaldiy" v-show="viewModal">
            <modal :isEdit="isEdit" @Success="ShowSuccess" ref="modify" @Modified="ShowModified"/>
        </div>
        <div class="flex flex-col justify-between w-3/5">
            <div class="mb-10">
                <div>
                    <div class="input-group mb-3">
                        <input type="text" class="form-control" placeholder="Search" aria-label="Search" aria-describedby="basic-addon1" v-model="search">
                        <button class="bg-slate-300 px-3" @click="Search"> Search</button>
                    </div>
                </div>
            </div>
            <div class="row row-cols-1 row-cols-md-3 g-4" style="height: 60vh;overflow: auto;">
                <template v-for="(items, index) in data" :key="index">
                    <div class="col">
                        <div class="card" style="height: 35vh;overflow: hidden;">
                            <img :src="items.image " class="card-img-top mx-44 my-10" alt="..." style="width:65px;height: 65px;">
                            <div class="card-body">
                                <h5 class="card-title font-extrabold">{{  items.title.length > 50 ? items.title.slice(0, 50)+"..." : items.title }}</h5>
                                <p class="card-text text-wrap" style="height: 10vh;">{{ items.description.slice(0, 200)+"..." }}</p>
                                <div class="flex justify-end my-2">
                                    <button class="btn btn-secondary mx-2 px-3" @click="onEdit(items)">Edit</button>
                                    <button class="btn btn-danger mx-2 px-3" @click="onDelete(items.id)">Delete</button>
                                </div>
                            </div>
                        </div>
                    </div>
                </template>
                <div class="col">
                    <div class="card flex justify-center items-center" style="height: 35vh;overflow: hidden;">
                        <button class="w-full h-full" @click="viewModal=true">
                            <img src="https://media.istockphoto.com/id/688550958/fr/vectoriel/signe-plus-noir-symbole-positif.jpg?s=612x612&w=0&k=20&c=o0v7a-kSIQt8fjaeiUATtnv-gKfVlRwf7SeuBLhbpRE=" class="card-img-top mx-44 my-10" alt="..." style="width:65px;height: 65px;">
                        </button>
                        </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import modal from '../modal/index.vue';

export default{
    components:{
        modal,
    },
    data(){
        return{
            header: [],
            data: [],
            search: "",
            viewModal: false,
            ShowSuccessModal: false,
            isEdit: false,
            modified: false,
            isDeleted: false,
        }
    },
    mounted(){
        this.fetchData();
    },
    methods:{
        ShowSuccess(val){
            this.fetchData();
            this.viewModal = false;
            this.ShowSuccessModal = true;
            this.modified = true;
            
            setTimeout(function(){
                this.ShowSuccessModal = false;
                console.log("Timeout!");
            }, 5000);
        },
        ShowModified(){
            this.fetchData();
            this.viewModal = false;
            this.ShowSuccessModal = true;
            
            setTimeout(function(){
                this.ShowSuccessModal = false;
                console.log("Timeout!");
            }, 5000);
        },
        async fetchData(filter = ""){
            return await fetch('https://fakestoreapi.com/products')
            .then(res=>res.json())
            .then((json) => {
                if(filter.length){
                    this.data = []
                    json.map(element => {
                        if(element.title.split(" ").find((element) => element == this.search)){
                            this.data.push(element);
                        }
                    })
                }
                else{
                    this.data = json
                }
                this.header = Object.keys(json[0])
            })
        },
        Search(){
            this.fetchData(this.search)
        },
        onEdit(val){
            this.viewModal = true;
            this.isEdit = true;
            this.$refs.modify.fetchData(val);
        },
        async onDelete(id){
            await fetch('https://fakestoreapi.com/products/'+id,{
                method:"DELETE"
            })
            .then(res=>res.json())
            .then(json=>{
                this.isDeleted = true;
                this.ShowSuccessModal = true;
            })
        }
    },
}
</script>

<style>
.dimBg{
    z-index: 1;
    position: absolute;
    width: 100%;
    height:100vh;
    background: rgba(0,0,0,0.3);
    cursor: pointer;
}
.modaldiy{
    z-index: 2;
}
</style>