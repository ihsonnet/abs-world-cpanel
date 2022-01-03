<template>
    <div>
    <v-card rounded="0" elevation="0" color="#f2f5f8"> 
            <v-breadcrumbs :items="items">
            <template v-slot:divider>
                <v-icon>mdi-chevron-right</v-icon>
            </template>
            </v-breadcrumbs>
        </v-card>
    <v-container>
        <v-snackbar
        timeout="2000"
        :value="successBar"
        absolute
        top
        right
        color="success accent-4"
        >
        {{ message }}
        </v-snackbar>
         <v-snackbar
        timeout="2000"
        :value="errorBar"
        absolute
        top
        right
        color="error accent-4"
        >
        {{ message }}
        </v-snackbar>
        <!-- category list  -->
                   <v-row>
                       <v-col>
                           <v-card class="pa-4 mt-2" elevation="0" style="border: 1px solid #e7e7e7" width="100%">
                               <v-row class="pa-5">
                                   <v-icon large>mdi-timetable</v-icon> <h3 class="mt-1 ml-2">Categories</h3>   <v-spacer></v-spacer> <v-btn depressed @click="createCatDialog = true" color="info">Create Category</v-btn>
                               </v-row>
                               <v-row style="background-color:#f2f5f8;border-radius:8px;text-align:center">
                                   <v-col cols="2">
                                       <b>Image</b>
                                   </v-col>
                                   <v-col>
                                       <b>Category Name</b>
                                   </v-col>
                                   <v-col>
                                       <b>Slug</b>
                                   </v-col>
                                   <v-col>
                                       <b>Action</b>
                                   </v-col>
                               </v-row>
                               <v-row v-for="cat in categoryList" :key="cat.catId" style="text-align:center;border-bottom: 1px solid #e7e7e7">
                                    <v-col class="mt-1" style="text-align:center" cols="2">
                                               <v-img class="mx-auto" style="border: 1px solid gray !important;" height="50" width="50" :src="cat.catImage"></v-img>
                                    </v-col>
                                    <v-col style="text-align:left">
                                        <h4 class="mt-5">
                                            {{cat.catName}}
                                        </h4> 
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            <v-chip color="info" small>{{cat.catSlug}}</v-chip>
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-btn-toggle class="mt-3">
                                                <v-btn color="info"  depressed small><v-icon style="color:white !important" small>mdi-pencil</v-icon></v-btn>
                                                <v-btn color="red" depressed small><v-icon style="color:white !important" small>mdi-delete</v-icon></v-btn>
                                        </v-btn-toggle>
                                    </v-col>
                               </v-row>
                           </v-card>
                       </v-col>
                   </v-row>   

                   <!-- dialog here -->

    <v-dialog title="Create New Category" v-model="createCatDialog" max-width="500px">
        <v-card class="pa-5">
            <h3>Create New Category</h3>
            <br>
            <v-form>
                <v-row>
                    <v-col>
                        <v-text-field
                            v-model="createCatModel.catName"
                            label="Category Name"
                            required
                            outlined
                            dense
                        ></v-text-field>

                        <v-row>
                            <v-col cols="8">
                                <v-file-input
                                    accept="image/*"
                                    label="File input"
                                    v-model="catImageLink"
                                ></v-file-input>
                            </v-col>
                            <v-col>
                                <v-img
                                height="80"
                                width="80"
                                style="border:1px solid gray"
                                :src="'@'+catImageLink"
                                >

                                </v-img>
                            </v-col>
                        </v-row>
                        <br>
                        <v-btn depressed @click="createCategory" color="info"><v-icon class="mr-2">mdi-content-save</v-icon> Create</v-btn>
                </v-col>
                </v-row>
            </v-form>
        </v-card>
    </v-dialog>
            
    </v-container>
</div>  
</template>

<script>
import axios from "axios"
export default {
  data () {
    return {
        CATEGORY_API: "https://abs-world-xpress.herokuapp.com/api/category/",
        auth: "Bearer " + localStorage.getItem("token"),
        createCatDialog: false,
        successBar: false,
        errorBar: false,
        message: "No Messsage",
        categoryList: [],
        createCatModel: {
            catName: ""
        },
        catImagefile: "",
        items: [
            {
            text: 'a2sDMS',
            disabled: false,
            href: '/',
            },
            {
            text: 'Category Controller',
            disabled: true,
            href: 'category-controller',
            },
        ]
    }
  },
  methods: {
      show () {
        return 0
    },
    getRandomColor() {
        return 'rgb(' + 
            (Math.floor(Math.random()*56)+200) + ', ' +
            (Math.floor(Math.random()*56)+200) + ', ' +
            (Math.floor(Math.random()*56)+200) +
            ')';
    },
    getCategoryList(){
        axios({
            method: "get",
            url: this.CATEGORY_API,
        })
        .then(r => {
            this.categoryList = r.data.data;
                })
        .catch(r => {
            console.log(r)
        });
    },
    createCategory(){
        axios({
            method:"POST",
            url: this.CATEGORY_API,
            data: this.createCatModel,
            headers: {
                Authorization: this.auth,
                "Content-Type": "application/json"
            }
        })
        .then(r=>{
            if(r.data.statusCode==201){
                this.uploadCatImage(r.data.data.catId);
                this.message = "Category Added!"
                this.successBar = true;
                this.createCatDialog = false
                this.getCategoryList();
            }
            else {
                this.message = "Something wrong!"
                this.errorBar = true;
            }
        });

    },
    uploadCatImage(id){
        let formData = new FormData();
        formData.append("image", this.catImageFile);

        console.log(formData)
        axios({
            method:"POST",
            url: this.CATEGORY_API+'image/'+id,
            data: formData,
            headers: {
                Authorization: this.auth,
                "Content-Type": "multipart/form-data"
            }
        })
        .then(r=>{
            console.log(r);
        });
    },
  },
  mounted(){
      this.getCategoryList();
  }
}
</script>
