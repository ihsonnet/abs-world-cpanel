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
        <!-- product list  -->
                   <v-row>
                       <v-col>
                           <v-card class="pa-4 mt-2" elevation="0" style="border: 1px solid #e7e7e7" width="100%">
                               <v-row class="pa-5">
                                   <v-icon large>mdi-timetable</v-icon> <h3 class="mt-1 ml-2">Products List</h3>   <v-spacer></v-spacer> <v-btn depressed @click="createAppDialog = true" color="info">Add Product</v-btn>
                               </v-row>
                               <v-row style="background-color:#f2f5f8;border-radius:8px;text-align:center">
                                   <v-col cols="1">
                                       <b>Image</b>
                                   </v-col>
                                   <v-col>
                                       <b>Product Name</b>
                                   </v-col>
                                   <v-col>
                                       <b>Buying Price</b>
                                   </v-col>
                                    <v-col>
                                       <b>Regular Price</b>
                                   </v-col>
                                    <v-col>
                                       <b>Cashback</b>
                                   </v-col>
                                   <v-col>
                                       <b>Stock</b>
                                   </v-col>
                                   <v-col>
                                       <b>Action</b>
                                   </v-col>
                               </v-row>
                               <v-row justify="center" align="center" v-for="item in productList.productList" :key="item.productId" style="text-align:center;border-bottom: 1px solid #e7e7e7">
                                    <v-col class="mt-1" style="text-align:center" cols="1">
                                               <v-img class="mx-auto" style="border: 1px solid gray !important;" height="50" width="50" :src="item.productImage"></v-img>
                                    </v-col>
                                    <v-col>
                                        <h4 justify="center">
                                            {{item.productName}}
                                        </h4> 
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            BDT {{item.buyingPrice}}
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            BDT {{item.regularPrice}}
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            BDT {{item.cashBack}}
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            <v-chip color="info" small>{{item.stockAvailable}}</v-chip>
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-btn-toggle class="mt-3">
                                                <v-btn color="info"  depressed small><v-icon style="color:white !important" small>mdi-pencil</v-icon></v-btn>
                                                <v-btn color="success"  depressed small><v-icon style="color:white !important" small>mdi-plus</v-icon></v-btn>
                                                <v-btn color="red" @click="deleteItemDialog=true" depressed small><v-icon style="color:white !important" small>mdi-delete</v-icon></v-btn>
                                        </v-btn-toggle>
                                    </v-col>
                               </v-row>
                           </v-card>
                       </v-col>
                   </v-row>   

                   <!-- dialog here -->

    <v-dialog title="Add New Drug" v-model="createAppDialog" max-width="900px">
        <v-card class="pa-5">
            <h3>Add New Product</h3> <br>
            <v-row>
                <v-col>
                    <v-select
                        v-model="createProductModel.categoryId"
                        label="Select Category"
                        :items="categoryList"
                        item-text="catName"
                        item-value="catId"
                        required
                        outlined
                        dense
                    ></v-select>
                    <v-text-field
                        v-model="createProductModel.productName"
                        label="Product Name"
                        required
                        outlined
                        dense
                    ></v-text-field>
                    <v-textarea
                        v-model="createProductModel.productDescription"
                        label="Product Description"
                        required
                        outlined
                        dense
                    ></v-textarea>
                </v-col>
                <v-col>
                    <v-row>
                        <v-col cols="8">
                            <label for="uoloadImage">Upload Product Image:</label><br><br>
                            <input
                                type="file"
                                name="uoloadImage"
                                accept="image/*"
                                @change="selectImage"
                            >
                        </v-col>
                        <v-col>
                            <v-img
                            height="80"
                            width="80"
                            style="border:1px solid gray"
                            :src="imageLink"
                            >

                            </v-img>
                        </v-col>
                    </v-row>
                    <br>
                    <v-text-field
                        v-model="createProductModel.buyingPrice"
                        label="Buying Price"
                        required
                        outlined
                        dense
                    ></v-text-field>
                    <v-text-field
                        v-model="createProductModel.regularPrice"
                        label="Regular Selling Price"
                        required
                        outlined
                        dense
                    ></v-text-field>
                    <v-text-field
                        v-model="createProductModel.cashBack"
                        label="CashBack / Price Cut"
                        required
                        outlined
                        dense
                    ></v-text-field>
                    <v-text-field
                        v-model="createProductModel.stockAvailable"
                        label="Initial Sock"
                        required
                        outlined
                        dense
                    ></v-text-field>
                </v-col>
            </v-row>
            
            <v-btn @click="save" depressed color="info"><v-icon class="mr-2">mdi-content-save</v-icon> Save Product</v-btn>
        </v-card>
    </v-dialog>

    <v-dialog title="Add New Drug" v-model="deleteItemDialog" max-width="350px">
        <v-card class="pa-5 text-center">
            <h3>Are you want delete this Product?</h3> <br>
            <v-row justify="center" align="center">
                <v-col>
                    <v-btn @click="deleteItemDialog=false" depressed color="info"><v-icon class="mr-2">mdi-cancel</v-icon> No</v-btn>
                </v-col>
                <v-col>
                    <v-btn @click="deleteItem(selectedItem)" depressed color="error"><v-icon class="mr-2">mdi-check</v-icon> Yes</v-btn>
                </v-col>
            </v-row>
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
        PRODUCT_API:"https://abs-world-xpress.herokuapp.com/api/product/",
        CATEGORY_API: "https://abs-world-xpress.herokuapp.com/api/category/",
        auth: "Bearer " + localStorage.getItem("token"),
        createAppDialog: false,
        deleteItemDialog: false,
        isEdit: false,
        successBar: false,
        errorBar: false,
        message: "No Messsage",
        productList: [],
        categoryList: [],
        createProductModel: {
            buyingPrice: 0,
            cashBack: 0,
            categoryId: "",
            currentPrice: 0,
            productDescription: "",
            productName: "",
            regularPrice: 0,
            stockAvailable: 0
        },
        productImageLink: "",
        imageFile: null,
        imageLink: "",
        selectedItem: null,
        items: [
            {
            text: 'a2sDMS',
            disabled: false,
            href: '/',
            },
            {
            text: 'Create Appointment',
            disabled: true,
            href: 'create-appointment',
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
    getProductList(){
        axios({
            method: "get",
            url: this.PRODUCT_API+`?orderBy=ASC&pageNo=0&pageSize=20&sortBy=creationTime`,
        })
        .then(r => {
            this.productList = r.data.data;
            console.log(this.productList.productList[0].productName)
                })
        .catch(r => {
            console.log(r)
        });
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
    save(){
        this.errorBar = false;
        this.successBar = false;
        this.createProductModel.currentPrice = this.createProductModel.regularPrice - this.createProductModel.cashBack;
        if(this.isEdit){
            this.editProduct();
        }
        else{
            this.addNewProduct();
        }
    },
    addNewProduct(){
        axios({
            method:"POST",
            url: this.PRODUCT_API+`cpanel/`,
            data: this.createProductModel,
            headers: {
                Authorization: this.auth,
                "Content-Type": "application/json"
            }
        })
        .then(r=>{
            if(r.data.statusCode==201){
                this.uploadImage(r.data.data.productId);
                this.message = "Product Added!"
                this.successBar = true;
                this.createAppDialog = false
                this.getProductList();
            }
            else {
                this.message = "Something wrong!"
                this.errorBar = true;
            }
        })
        .catch(e=>{
            this.message = e;
            this.errorBar = true;
        });
    },
    selectImage(event){
        this.imageFile = event.target.files[0];
        console.log(this.imageFile);
        if (this.imageFile) {
            this.imageLink = URL.createObjectURL(this.imageFile)
        }
    },
    uploadImage(id){
        let formData = new FormData();
        formData.append("image", this.imageFile, this.imageFile.name);
        console.log(formData);

        axios({
            method:"POST",
            url: this.PRODUCT_API+'cpanel/image/'+id,
            data: formData,
            headers: {
                Authorization: this.auth,
                "Content-Type": "multipart/form-data"
            }
        })
        .then(r=>{
            console.log(r);
            this.getProductList();
        });
    },
    editProduct(){

    },
  },
  mounted(){
      this.getProductList();
      this.getCategoryList();
  }
}
</script>
