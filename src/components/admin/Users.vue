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

        <!-- appointment list  -->
                   <v-row>
                       <v-col>
                           <v-card class="pa-4 mt-2" elevation="0" style="border: 1px solid #e7e7e7" width="100%">
                               <v-row class="pa-5">
                                   <v-icon large>mdi-timetable</v-icon> <h3 class="mt-1 ml-2">Users</h3>   <v-spacer></v-spacer> <v-btn depressed @click="createAppDialog = true" color="info">Register User</v-btn>
                               </v-row>
                               <v-row style="background-color:#f2f5f8;border-radius:8px;text-align:center">
                                   <v-col cols="4">
                                       <b>Name</b>
                                   </v-col>
                                   <v-col>
                                       <b>Email</b>
                                   </v-col>
                                   <v-col>
                                       <b>Phone Number</b>
                                   </v-col>
                                   <v-col>
                                       <b>Address</b>
                                   </v-col>
                                   <v-col>
                                       <b>Action</b>
                                   </v-col>
                               </v-row>
                               <v-row justify="center" align="center" v-for="user in customerList" :key="user.id" style="text-align:center;border-bottom: 1px solid #e7e7e7">
                                    <v-col class="ml-2" style="text-align:left" cols="4">
                                        <v-row>
                                            <v-col cols="3">
                                               <v-avatar
                                               class="white--text"
                                                :color="getRandomColor()"
                                                size="42"
                                                ><h3>{{user.fullName.charAt(0)}}</h3></v-avatar>
                                            </v-col>
                                            <v-col>
                                                 <h4>
                                                    {{user.fullName}}
                                                </h4> 
                                                <v-chip x-small><b>@{{user.username}}</b></v-chip>  
                                            </v-col>
                                        </v-row>
                                    </v-col>
                                    <v-col>
                                         <v-card-subtitle>
                                            {{user.email}}
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            {{user.phoneNo}}
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            {{user.deliveryAddress}}
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-btn-toggle>
                                                <v-btn color="info" @click="readyForEdit(item),isEdit = true,createAppDialog=true" depressed small><v-icon style="color:white !important" small>mdi-pencil</v-icon></v-btn>
                                                <v-btn color="success"  depressed small><v-icon style="color:white !important" small>mdi-eye</v-icon></v-btn>
                                                <v-btn color="red" @click="deleteItemDialog=true ,selectedItem=item" depressed small><v-icon style="color:white !important" small>mdi-block-helper</v-icon></v-btn>
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
import axios from 'axios'
export default {
  data () {
    return {
        USER_API: 'http://194.233.68.154:8082/api/customers/',
        auth: "Bearer " + localStorage.getItem("token"),
        createAppDialog: false,
        deleteItemDialog: false,
        loader: false,
        isEdit: false,
        successBar: false,
        errorBar: false,
        message: "No Messsage",
        pageNo: 0,
        customerList: [],
        appointmentList: [
                {
                    id:"1",
                    firstName: "Injamamul Haque",
                    lastName:"Sonet",
                    age:"22",
                    phoneNo: "017354635920",
                    address:"Dhaka, Bangladesh"
                },
                {
                    id:"2",
                    firstName: "Faisul",
                    lastName:"Islam",
                    age:"19",
                    phoneNo: "017354635920",
                    address:"Dhaka, Bangladesh"
                }
            ],
        items: [
            {
            text: 'Home',
            disabled: false,
            href: '/',
            },
            {
            text: 'User Controller',
            disabled: true,
            href: 'users',
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
    getUserList(){
        this.loader = true;
        axios({
            method: "get",
            url: this.USER_API+`?orderBy=ASC&pageNo=0&pageSize=20&sortBy=createdOn`,
            headers: {
                Authorization: this.auth
            }
        })
        .then(r => {
            this.loader = false;
            this.customerList = r.data.data.customerList;
            console.log(this.customerList)
                })
        .catch(r => {
            console.log(r)
        });
    },
  },
  mounted() {
      this.getUserList()
  }
}
</script>
