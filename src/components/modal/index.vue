<template>
   <div class="diyModal card" style="width: 30em;">
        <div class="card-header text-lg font-extrabold">
            {{ isEdit ? "Edit" : "Add" }}
        </div>
        <div class="card-body">
            <div class="mb-3">
                <label for="exampleInputEmail1" class="form-label">Image</label>
                <input class="form-control" type="file" id="formFile" @change="UploadFile">
            </div>
            <div class="mb-3">
                <label class="form-label">Title</label>
                <input type="text" class="form-control" v-model="model.title">
            </div>
            <div class="mb-3">
                <label class="form-label">Price</label>
                <div class="input-group mb-3">
                    <span class="input-group-text" id="basic-addon1">$</span>
                    <input type="number" class="form-control" aria-describedby="basic-addon1" v-model="model.price">
                </div>

            </div>
            <div class="mb-3">
                <label class="form-label">Description</label>
                <input type="text" class="form-control" v-model="model.description">
            </div>
            <div class="mb-3">
                <label class="form-label">Category</label>
                <input type="text" class="form-control" v-model="model.category">
            </div>
            <div class="flex justify-end">
                <template v-if="!isEdit">
                    <button type="submit" class="btn btn-primary mx-2" @click="AddData">Save changes</button>
                </template>
                <template v-else>
                    <button type="submit" class="btn btn-primary mx-2" @click="editData">Save changes</button>
                </template>
            </div>
        </div>
    </div>
</template>

<script>

export default{
    props:['isEdit'],
    data(){
        return{
            model: {},
        }
    },
    methods:{
        fetchData(val){
            this.model = val;
        },
        UploadFile(e){
            var files = e.target.files || e.dataTransfer.files;
            if (!files.length)
                return;
            this.model.image = files[0].name;
        },
        async AddData(){
            await fetch('https://fakestoreapi.com/products',{
            method:"POST",
                body:JSON.stringify(this.model)
            })
            .then(res=>res.json())
            .then(json=>{
                this.$emit('Success', json);
            })
        },
        async editData(){
            await fetch('https://fakestoreapi.com/products/'+ this.model.id ,{
                method:"PATCH",
                body:JSON.stringify(this.model)
            })
            .then(res=>res.json())
            .then(json=>{
                this.$emit('Modified', json);
            })
        }
    }
}
</script>
<style>
.diyModal{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
</style>