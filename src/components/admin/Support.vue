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
                       <v-col cols="4">
                        <v-card class="pa-4 mt-2" elevation="0" style="border: 1px solid #e7e7e7" width="100%">
                               <v-row class="pa-2">
                                    <v-col>
                                        <v-row class="pa-3">
                                            <v-icon large>mdi-chat</v-icon> <h3 class="mt-1 ml-2">Tickets List  <v-chip color="info" x-small>{{response.ticketCount}}</v-chip></h3>
                                        </v-row>
                                    </v-col>
                                    <v-col>
                                        <v-text-field
                                        append-icon="mdi-magnify"
                                        @click:append="getOrderList()"
                                        v-model="searchText"
                                        label="Search"
                                        outlined
                                        dense
                                        style="width:200px !important;"
                                    ></v-text-field> 
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
                               <v-row :class="ticket.ticketSKU==selectedTicketSKU? 'selected':'line'" @click="selectedTicketSKU=ticket.ticketSKU" v-for="ticket in ticketlist" :key="ticket.ticketId">
                                    <v-col class="ml-2" style="text-align:left" cols="4">
                                            <h4 class="mt-3">
                                            #{{ticket.ticketSKU}}
                                        </h4>   
                                        <h5>
                                           <v-icon small>mdi-label</v-icon> {{ticket.ticketTitle}}
                                        </h5>

                                        <small>By @{{ticket.createdBy}}</small>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle class="mt-5">
                                            <v-chip v-if="ticket.isOpen" color="success" small>Open</v-chip>
                                            <v-chip v-if="!ticket.isOpen" small>Closed</v-chip>
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            Last Update: {{ticket.updatedTime}}<br>
                                            By <v-chip x-small>{{ticket.updatedBy}}</v-chip>
                                        </v-card-subtitle>
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
                       <v-col>
                           <v-card class="pa-4 mt-2" elevation="0" style="border: 1px solid #e7e7e7" width="100%" height="800px">
                                    <v-row class="pa-8" card>
                                        <h3 class="mt-1 ml-2">#TID234667</h3>   <v-spacer></v-spacer> <v-btn depressed @click="createAppDialog = true" outlined color="success"><v-icon>mdi-check</v-icon> Mark As Complete</v-btn>
                                    </v-row>

                                    <v-divider></v-divider>

                                    <v-card-text>
                                        <v-row :v-for="ticket in ticketlist">
                                        
                                            {{ 1 }} <br>
                                            {{ 1 }} <br>
                                            {{ 1 }} <br>
                                            {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              {{ 1 }} <br>
                                              
                                        </v-row>
                                        <v-footer>
                                        <div>Footer</div>
                                        </v-footer>
                                    </v-card-text>
                           </v-card>
                       </v-col>
                   </v-row> 

                   <!-- dialog here -->

    <v-dialog title="Add New Drug" v-model="createAppDialog" max-width="800px">
        <v-card class="pa-5">
            
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
        SUPPORT_API: 'http://194.233.68.154:8082/api/support',
        auth: "Bearer " + localStorage.getItem("token"),
        createAppDialog: false,
        deleteItemDialog: false,
        loader: false,
        isEdit: false,
        successBar: false,
        errorBar: false,
        message: "No Messsage",
        pageNo: 0,
        imageLink:'',
        imageFile: null,
        searchText: '',
        ticketlist: [],
        selectedTicketSKU: '',
        selectedTicket: {},
        response: {},
        settingsModel: {
                "description": "",
                "facebookPage": "",
                "imo": "",
                "isServerDown": true,
                "isUnderMaintenance": true,
                "maintenanceMsg": "",
                "serverDownMsg": "",
                "shortDescription": "",
                "telegramLink": "",
                "whatsapp": "",
                "youtubeChannel": ""
                },
        items: [
            {
            text: 'Home',
            disabled: false,
            href: '/',
            },
            {
            text: 'Support',
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
    getSliders(){
         axios({
            method: "get",
            url: this.SLIDERS_API
        })
        .then(r => {
            this.sliderList = r.data;
            console.log(this.sliderList)
        })
        .catch(r => {
            console.log(r)
        });
    },
    getTickets(){
        axios({
            method: "get",
            url: this.SUPPORT_API+`?orderBy=DESC&pageNo=${this.pageNo}&pageSize=5&sortBy=creationTime`,
            headers: {
                Authorization: this.auth
            }
        })
        .then(r => {
            var support = r.data.data;
            this.response = support;
            this.ticketlist = support.ticketList;
            this.selectedTicketSKU = this.ticketlist[0].ticketSKU;

            console.log(support)
                })
        .catch(r => {
            console.log(r)
        });
    },
        selectImage(event){
        this.imageFile = event.target.files[0];
        console.log(this.imageFile);
        if (this.imageFile) {
            this.imageLink = URL.createObjectURL(this.imageFile)
        }
    },
    
    editSettings(){
        console.log("Edit call")
        console.log(this.settingsModel);
        axios({
            method:"PUT",
            url: this.SETTINGS_API,
            data: this.settingsModel,
            headers: {
                Authorization: this.auth,
                "Content-Type": "application/json"
            }
        })
        .then(r=>{
            if(r.data.statusCode==200){
                if(this.imageFile){
                    this.uploadLogo();
                }
                this.message = "Settings Edited!"
                this.successBar = true;
                this.createAppDialog = false
                this.getAppSettings();
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
  },
  mounted() {
      this.getTickets()
  }
}
</script>
<style>
.line {
    text-align:center;border-bottom: 1px solid #e7e7e7;
}
.line:hover {
    background-color:#f2f5f8;border-radius:15px;text-align:center;cursor: pointer;
}
.selected {
    background-color:#e2e0e0;border-radius:15px;text-align:center;cursor: pointer;
}

.v-card {
  display: flex !important;
  flex-direction: column;
}

.v-card__text {
  flex-grow: 1;
  overflow: auto;
}
</style>
