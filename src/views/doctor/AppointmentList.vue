<template>
    <v-container>

        <div>

        <!-- appointment list  -->
                   <v-row>
                       <v-col>
                           <v-card class="pa-4 mt-2" elevation="0" style="border: 1px solid #e7e7e7" width="100%">
                               <v-row class="pa-5">
                                   <v-icon large>mdi-clipboard-check</v-icon> <h3 class="mt-1 ml-2">Appointment List</h3> 
                               </v-row>
                               <v-row style="background-color:#f2f5f8;border-radius:8px;text-align:center">
                                   <v-col cols="4">
                                       <b>Name</b>
                                   </v-col>
                                   <v-col>
                                       <b>Age</b>
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
                                                 <h4 class="mt-5">
                                                    {{appointment.firstName}} {{appointment.lastName}}
                                                </h4>   
                                            </v-col>
                                        </v-row>
                                    </v-col>
                                    <v-col>
                                        <v-chip class="mt-3" small outlined color="teal">{{appointment.age}}</v-chip>
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
                                                <v-btn color="info" depressed small><v-icon small>mdi-prescription</v-icon></v-btn>
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

    </div>         
            
    </v-container>
</template>

<script>
export default {
  data () {
    return {
        AppDetailsDialog: false,
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
            text: 'a2sDMS',
            disabled: false,
            href: '/',
            },
            {
            text: 'Appointment List',
            disabled: true,
            href: 'appointment-list',
            },
        ],
    }
  },
  methods: {
    show () {
      return 0;
    },
    getRandomColor() {
        return 'rgb(' + 
            (Math.floor(Math.random()*56)+200) + ', ' +
            (Math.floor(Math.random()*56)+200) + ', ' +
            (Math.floor(Math.random()*56)+200) +
            ')';
        },
  }
}
</script>
