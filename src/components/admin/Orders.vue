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

        <!-- order list  -->

                   <v-row>
                       <v-col>
                           <v-card class="pa-4 mt-2" elevation="0" style="border: 1px solid #e7e7e7" width="100%">
                               <v-row class="pa-5">
                                    <v-col>
                                        <v-row>
                                            <v-icon large>mdi-clipboard-text</v-icon> <h3 class="mt-1 ml-2">Order List</h3>
                                        </v-row>
                                    </v-col>
                                   <v-spacer></v-spacer> 
                                       <v-text-field
                                        append-icon="mdi-magnify"
                                        @click:append="getOrderList()"
                                        v-model="searchText"
                                        label="Search"
                                        outlined
                                        dense
                                        class="mr-2"
                                        style="width:200px !important;"
                                    ></v-text-field> 
                               </v-row>
                               <v-row style="background-color:#f2f5f8;border-radius:8px;text-align:center">
                                   <v-col cols="4">
                                       <b>Order ID</b>
                                   </v-col>
                                   <v-col>
                                       <b>Phone Number</b>
                                   </v-col>
                                   <v-col>
                                       <b>Ammount</b>
                                   </v-col>
                                   <v-col>
                                       <b>Status</b>
                                   </v-col>
                                   <v-col>
                                       <b>Action</b>
                                   </v-col>
                               </v-row>
                                <v-row v-if="loader" align="center" justify="center" class="text-center">
                                   <v-col>
                                       <v-progress-circular
                                        :size="50"
                                        color="primary"
                                        indeterminate
                                        ></v-progress-circular>
                                   </v-col>
                               </v-row>
                               <v-row v-for="order in orderList" :key="order.id" style="text-align:center;border-bottom: 1px solid #e7e7e7">
                                    <v-col class="ml-2" style="text-align:left" cols="4">

                                        <v-row>
                                            <v-col cols="3">
                                               <v-avatar
                                               class="ma-3 white--text"
                                                :color="getRandomColor()"
                                                size="42"
                                                ><h3><v-icon>mdi-cart</v-icon></h3></v-avatar>
                                            </v-col>
                                            <v-col>
                                                 <h4 class="mt-3">
                                                    #{{order.orderSKU}}
                                                </h4>   
                                                <small>By {{order.customerName}}</small>
                                            </v-col>
                                        </v-row>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            {{order.customerPhoneNumber}}
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            BDT {{order.orderAmount}} <br>
                                            <v-chip :color="order.paymentStatus=='paid' ? 'success': ''" x-small>{{order.paymentStatus}}</v-chip>
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            {{order.orderStatus}}
                                        </v-card-subtitle>
                                    </v-col>
                                    
                                    <v-col>
                                        <v-btn-toggle class="mt-3">
                                                <v-btn color="info"  depressed small><v-icon style="color:white !important" small>mdi-eye</v-icon></v-btn>
                                                <v-btn color="info" depressed small><v-icon style="color:white !important" small>mdi-pencil</v-icon></v-btn>
                                        </v-btn-toggle>
                                    </v-col>
                               </v-row>
                               <v-row>
                                   <v-col style="text-align:center">
                                       <v-btn :disabled="pageNo==0" @click="pageNo=pageNo-1" class="mr-2" depressed color="error">
                                           Previous
                                       </v-btn>
                                       <v-btn :disabled="response.lastPage" depressed @click="pageNo=pageNo+1" color="info">
                                           Next
                                       </v-btn>
                                   </v-col>
                               </v-row>
                           </v-card>
                       </v-col>
                   </v-row>            

                   <!-- dialog here -->

    <v-dialog title="Add New Drug" v-model="AppDetailsDialog" max-width="800px">
        <v-card class="pa-5">
            <h3>Add New Drug</h3>
            
            <v-btn depressed color="info"><v-icon class="mr-2" @click="AppDetailsDialog = false">mdi-content-save</v-icon> Save Drug</v-btn>
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
        ORDER_API: "http://194.233.68.154:8082/api/order/",
        auth: "Bearer " + localStorage.getItem("token"),
        AppDetailsDialog: false,
        pageNo: 0,
        response: '',
        loader: false,
        searchText: '',
        phoneNumber:null,
        sku: null,
        getUrl:'',
        orderList: [],
        items: [
            {
            text: 'Home',
            disabled: false,
            href: '/',
            },
            {
            text: 'Orders',
            disabled: true,
            href: 'orders',
            },
        ],
    }
  },
  methods: {
    show () {
      return 0;
    },
    searchChecking(text){
        if(text.startsWith('01')) this.getUrl = this.ORDER_API+`?customerPhoneNumber=${this.searchText}&orderBy=DESC&${this.pageNo}&pageSize=20&sortBy=creationTime`;
        if(text.startsWith("#")) this.getUrl = this.ORDER_API+`?orderBy=DESC&orderSKU=${this.searchText.slice(1)}&${this.pageNo}&pageSize=20&sortBy=creationTime`
    },
    getOrderList(){
        this.loader = true;
        if(this.searchText) this.searchChecking(this.searchText)
        else this.getUrl = this.getUrl = this.ORDER_API+`?orderBy=DESC&${this.pageNo}&pageSize=20&sortBy=creationTime`
        
        axios({
            method: "get",
            url: this.getUrl,
            headers: {
            Authorization: this.auth,
            "Content-Type": "application/json"
            }
        })
        .then(r => {
            this.loader = false;
            this.orderList = r.data.data.orderList;
            this.response = r.data.data;
            console.log(this.orderList);
        })
        .catch(r => {
            console.log(r)
        });
    },
    getRandomColor() {
        return 'rgb(' + 
            (Math.floor(Math.random()*56)+200) + ', ' +
            (Math.floor(Math.random()*56)+200) + ', ' +
            (Math.floor(Math.random()*56)+200) +
            ')';
        },
  },
   mounted(){
      this.getOrderList();
  },
  watch: {
      pageNo : function(){
          this.getOrderList();
      },
      searchText : function (){
        this.getOrderList();
      }
  }
}
</script>
