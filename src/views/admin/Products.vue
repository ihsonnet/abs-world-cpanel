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
                               <v-row v-for="item in productList.productList" :key="item.productId" style="text-align:center;border-bottom: 1px solid #e7e7e7">
                                    <v-col class="mt-1" style="text-align:center" cols="1">
                                               <v-img class="mx-auto" style="border: 1px solid gray !important;" height="50" width="50" :src="item.productImage"></v-img>
                                    </v-col>
                                    <v-col>
                                        <h4 class="mt-5">
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
                                                <v-btn color="red" depressed small><v-icon style="color:white !important" small>mdi-delete</v-icon></v-btn>
                                        </v-btn-toggle>
                                    </v-col>
                               </v-row>
                           </v-card>
                       </v-col>
                   </v-row>   

                   <!-- dialog here -->

    <v-dialog title="Add New Drug" v-model="createAppDialog" max-width="800px">
        <v-card class="pa-5">
            <h3>Add New Drug</h3>
            
            <v-btn depressed color="info"><v-icon class="mr-2" @click="createAppDialog = false">mdi-content-save</v-icon> Save Drug</v-btn>
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
        createAppDialog: false,
        productList: [],
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
    }
  },
  mounted(){
      this.getProductList();
  }
}
</script>
