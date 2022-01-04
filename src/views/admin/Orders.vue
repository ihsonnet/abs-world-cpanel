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
                                   <v-icon large>mdi-clipboard-text</v-icon> <h3 class="mt-1 ml-2">Order List</h3>
                               </v-row>
                               <v-row style="background-color:#f2f5f8;border-radius:8px;text-align:center">
                                   <v-col cols="4">
                                       <b>Order ID</b>
                                   </v-col>
                                   <v-col>
                                       <b>Phone Number</b>
                                   </v-col>
                                   <v-col>
                                       <b>Address</b>
                                   </v-col>
                                   <v-col>
                                       <b>Amount</b>
                                   </v-col>
                                   <v-col>
                                       <b>Action</b>
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
                                            {{order.deliveryAddress}}
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            BDT {{order.orderAmount}}
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                                <v-btn color="info" depressed small><v-icon small>mdi-pencil</v-icon></v-btn>
                                        </v-card-subtitle>
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
        ORDER_API: "https://abs-world-xpress.herokuapp.com/api/order/",
        auth: "Bearer " + localStorage.getItem("token"),
        AppDetailsDialog: false,
        orderList: [],
        items: [
            {
            text: 'a2sDMS',
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
    getOrderList(){
        axios({
            method: "get",
            url: this.ORDER_API+'?orderBy=DESC&pageNo=0&pageSize=20&sortBy=creationTime',
            headers: {
            Authorization: this.auth,
            "Content-Type": "application/json"
            }
        })
        .then(r => {
            this.orderList = r.data.data.orderList;
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
  }
}
</script>
