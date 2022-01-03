<template>
    <div>
        <v-card rounded="0" elevation="0" color="#f2f5f8"> 
            <v-breadcrumbs :items="items">
            <template v-slot:divider>
                <v-icon>mdi-chevron-right</v-icon>
            </template>
            </v-breadcrumbs>
        </v-card>
        <v-row v-if="!userInfo" style="margin-top:20px;text-align:center;vertical-align:middle !important">
            <v-col style="margin:auto">
                <v-progress-circular
                :size="70"
                :width="7"
                color="#d4d6d8"
                indeterminate
                ></v-progress-circular>
            </v-col>
        </v-row>
        <v-row>
            <v-col lg="3" md="3" sm="12" cols="12">
                <v-card  flat  class="ma-5" elevation="0">
                   <v-row>
                        <v-col>
                            <v-card color="blue lighten-4" outlined class="mt-2 pa-4" elevation="0" style="border: 1px solid #e7e7e7" width="100%">
                                <v-row>
                                   <v-col class="mx-auto" style="text-align:center !important" align-self="center" cols="12">
                                       <v-img sizes="10" class="mx-auto" style="width:160px;height:160px;border:1px solid gray;border-radius:100px!important" src="https://pbs.twimg.com/profile_images/864282616597405701/M-FEJMZ0_400x400.jpg"></v-img>
                                        <br>
                                        <v-chip color="blue" class="white--text" small link><b>{{userInfo.username}}</b></v-chip> <br>
                                   </v-col>
                                   <v-col cols="12">
                                       <h2 class="ma-4"><b>{{ userInfo.firstName }} {{ userInfo.lastname }}</b></h2>
                                        <v-chip small outlined class="ml-4">{{userInfo.role}}</v-chip> <br>
                                        <v-card-subtitle>
                                                    <b>Email:</b> <br>{{userInfo.email}} <br>
                            
                                                    <b>Phone Number:</b> <br>{{userInfo.phone}} <br>
                                 
                                                    <b>Address:</b> <br> {{userInfo.address}} <br>
                                        </v-card-subtitle>
                                   </v-col>
                                </v-row>
                            </v-card>
                        </v-col>
                   </v-row>
                </v-card>
            </v-col>
            <v-divider
            class="mx-0"
            vertical
            ></v-divider>
            <v-col lg="" md="9" sm="12" cols="12">
                <v-card  class="ma-5" elevation="0">

                    <!-- dashboard -->

                     <h4>Dashboard</h4>
                    <br>
                    <v-row>
                        <v-col v-for="item in userInfo.dashboard" :key="item.text">
                           <v-card elevation="0" class="my-2" :color="getRandomColor()" link :to="'project/'+item.id">
                               <v-row>
                                   <v-col cols="4">
                                       <v-icon class="mx-5 my-7" x-large>{{item.icon}}</v-icon>
                                   </v-col>
                                   <v-col cols="8">
                                       <strong style="font-size:12;color:gray">{{item.text}}</strong> 
                                       <br>
                                       <h1 class="mt-4">{{item.count}}</h1>
                                   </v-col>
                               </v-row>
                           </v-card>
                        </v-col>
                    </v-row>
                   
                   
                   <!-- order list  -->
                   <v-row>
                       <v-col>
                           <v-card class="pa-4 mt-2" elevation="0" style="border: 1px solid #e7e7e7" width="100%">
                               <v-row class="pa-5">
                                   <v-icon large>mdi-clipboard-text</v-icon> <h3 class="mt-1 ml-2">Latest Orders</h3>
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
                               <v-row v-for="appointment in appointmentList" :key="appointment.id" style="text-align:center;border-bottom: 1px solid #e7e7e7">
                                    <v-col class="ml-2" style="text-align:left" cols="4">

                                        <v-row>
                                            <v-col cols="3">
                                               <v-avatar
                                               class="ma-3 white--text"
                                                :color="getRandomColor()"
                                                size="42"
                                                ><h3>{{appointment.firstName.charAt(0)}}</h3></v-avatar>
                                            </v-col>
                                            <v-col>
                                                 <h4 class="mt-3">
                                                    #Order0234
                                                </h4>   
                                                <small>By {{appointment.firstName}} {{appointment.lastName}}</small>
                                            </v-col>
                                        </v-row>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            {{appointment.phoneNo}}
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            {{appointment.address}}
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            BDT {{appointment.age}}000
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                                <v-btn color="info" depressed small><v-icon small>mdi-prescription</v-icon></v-btn>
                                        </v-card-subtitle>
                                    </v-col>
                               </v-row>
                           </v-card>
                       </v-col>
                   </v-row>            
                </v-card>
            </v-col>
        </v-row>

    </div>
</template>
<script>
import axios from "axios"
export default {
    data() {
        return {
            idx:0,
            input: "",
            adddialog: false,
            GET_LOGGED_IN_PROFILE_API: "https://buggie-backend.herokuapp.com/auth/user-info",
            user: {},
            auth: "Bearer " + localStorage.getItem("token"),
            userInfo: {
                firstName:"MD Sajib",
                lastname:"Hang",
                phone:"01734543027",
                username:"sajibhang@17",
                email:"sandra.dr@gmail.com",
                role:"ADMIN",
                address: "Dhaka, Bangladesh",
                dashboard: [
                {
                    text: "Todays Order",
                    count: 80,
                    icon: "mdi-playlist-minus"
                },
                {
                    text: "Todays Sales",
                    count: 800,
                    icon: "mdi-cash"
                },
                {
                    text: "Total Product",
                    count: 320,
                    icon: "mdi-playlist-check"
                },
                {
                    text: "Total Sales",
                    count: 34000,
                    icon: "mdi-cash-multiple"
                },
            ]
            },
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
                text: 'Dashboard',
                disabled: true,
                href: 'dashboard',
                },
            ],
            
        }
    },
    methods: {
        getProfileInfo() {
      console.log(this.auth)
      axios({
        method: "get",
        url: this.GET_LOGGED_IN_PROFILE_API,
        headers: {
          Authorization: this.auth,
          "Content-Type": "application/json"
        }
      })
        .then(r => {
        console.log(r.data)
        this.userInfo = r.data;
        localStorage.setItem("userInfo", JSON.stringify(r.data));
        this.userInfo = JSON.parse(localStorage.getItem("userInfo"));
        console.log(this.userInfo.firstName)
        // location.reload();
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
         show(){ 
            return 0   
        },
        
    },
    mounted(){
        // this.userInfo = JSON.parse(localStorage.getItem("userInfo"));
        // this.getProfileInfo();
    },
}
</script>
<style lang="scss" scoped>
.rowise .col-6{
    padding-bottom: 0px !important;
    padding-top: 0px !important;
}
</style>