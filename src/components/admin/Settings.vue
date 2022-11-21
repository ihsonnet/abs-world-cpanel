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
                                   <v-icon large>mdi-apple-finder</v-icon> <h3 class="mt-1 ml-2">App Settings</h3>   <v-spacer></v-spacer> <v-btn depressed @click="createAppDialog = true" color="error">Reset App</v-btn>
                               </v-row>
                               <v-row>
                                <v-col class="mx-auto" cols="8">
                                        <v-img
                                        width="250"
                                        style="border:1px solid gray"
                                        :src="imageLink"
                                        >
                                        </v-img>

                                        <label for="uoloadImage">Upload Your Logo:</label><br><br>
                                        <input
                                            type="file"
                                            name="uoloadImage"
                                            accept="image/*"
                                            @change="selectImage"
                                        >
                                </v-col>
                               </v-row>
                               <v-row>
                                <v-col>
                                    <v-card class="pa-4 mt-2" elevation="0" style="border: 1px solid #e7e7e7" width="100%">
                                        <v-checkbox
                                        v-model="settingsModel.isUnderMaintenance"
                                        label="Is your Application in Under maintance?"
                                        ></v-checkbox>
                                        <v-textarea
                                            v-model="settingsModel.maintenanceMsg"
                                            label="Maintanace Message"
                                            required
                                            outlined
                                            dense
                                            height="80"
                                        ></v-textarea>

                                        <v-checkbox
                                        v-model="settingsModel.isServerDown"
                                        label="Is your Server Down Now?"
                                        ></v-checkbox>
                                        <v-textarea
                                            v-model="settingsModel.serverDownMsg"
                                            label="Server Down Message"
                                            required
                                            outlined
                                            dense
                                            height="80"
                                        ></v-textarea>
                                    </v-card>
                                </v-col>
                                <v-col>
                                     <v-card class="pa-4 mt-2" elevation="0" style="border: 1px solid #e7e7e7" width="100%">
                                        <v-text-field
                                            v-model="settingsModel.facebookPage"
                                            label="Facebook Page"
                                            required
                                            outlined
                                            dense
                                            prepend-icon="mdi-facebook"
                                        ></v-text-field>
                                        <v-text-field
                                            v-model="settingsModel.youtubeChannel"
                                            label="YouTube Channel"
                                            required
                                            outlined
                                            dense
                                            prepend-icon="mdi-youtube"
                                        ></v-text-field>
                                        <v-text-field
                                            v-model="settingsModel.telegramLink"
                                            label="Telegram Channel"
                                            required
                                            outlined
                                            dense
                                            prepend-icon="mdi-label-outline"
                                        ></v-text-field>
                                        <v-text-field
                                            v-model="settingsModel.imo"
                                            label="Imo Number"
                                            required
                                            outlined
                                            dense
                                            prepend-icon="mdi-message-outline"
                                        ></v-text-field>
                                         <v-text-field
                                            v-model="settingsModel.whatsapp"
                                            label="WhatsApp Number"
                                            required
                                            outlined
                                            dense
                                            prepend-icon="mdi-whatsapp"
                                        ></v-text-field>
                                    </v-card>
                                </v-col>
                               </v-row>
                               <v-row>
                                <v-col>
                                     <v-card class="pa-4 mt-2" elevation="0" style="border: 1px solid #e7e7e7" width="100%">
                                        <v-row class="mt-2">
                                            <v-col>
                                                <v-textarea
                                                    v-model="settingsModel.shortDescription"
                                                    label="Short Description"
                                                    required
                                                    outlined
                                                    dense
                                                    height="100"
                                                ></v-textarea>
                                            </v-col>
                                            <v-col>
                                                <v-textarea
                                                    v-model="settingsModel.description"
                                                    label="Your App Description"
                                                    required
                                                    outlined
                                                    dense
                                                    height="100"
                                                ></v-textarea>
                                            </v-col>
                                        </v-row>
                                    </v-card>
                                </v-col>
                               </v-row>
                               <v-row>
                                <v-col cols="10">
                                    <span style="color:red">*Wrong settings can be harmfull for your application , it can effect on user experice and data loss. ao , be carefull before update/Reset app settings.</span>
                                </v-col>
                                <v-col class="text-right">
                                    <v-btn class="mx-2" depressed @click="editSettings()" color="info">Update</v-btn>
                                </v-col>
                               </v-row>
                           </v-card>
                       </v-col>
                   </v-row> 

                   <v-row>
                       <v-col>
                           <v-card class="pa-4 mt-2" elevation="0" style="border: 1px solid #e7e7e7" width="100%">
                               <v-row class="pa-5">
                                   <v-icon large>mdi-apple-finder</v-icon> <h3 class="mt-1 ml-2">Slides Settings</h3>   <v-spacer></v-spacer> <v-btn depressed @click="createAppDialog = true" color="info">Upload New Slider</v-btn>
                               </v-row>
                                 <v-row style="background-color:#f2f5f8;border-radius:8px;text-align:center">
                                   <v-col cols="1">
                                       <b>SL No</b>
                                   </v-col>
                                   <v-col cols="2">
                                       <b>Image</b>
                                   </v-col>
                                   <v-col cols="4">
                                       <b>Title</b>
                                   </v-col>
                                    <v-col>
                                       <b>Linker</b>
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
                               <v-row justify="center" align="center" v-for="item in sliderList.data" :key="item.id" style="text-align:center;border-bottom: 1px solid #e7e7e7">
                                    <v-col class="mt-1" style="text-align:center" cols="1">
                                               <v-img class="mx-auto" style="border: 1px solid gray !important;" height="50" width="50" :src="item.imageUrl"></v-img>
                                    </v-col>
                                    <v-col>
                                        <h4 justify="center">
                                           
                                        </h4> 
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-card-subtitle>
                                            <v-chip color="info" small></v-chip>
                                        </v-card-subtitle>
                                    </v-col>
                                    <v-col>
                                        <v-btn-toggle class="mt-3">
                                                <v-btn color="info" depressed small><v-icon style="color:white !important" small>mdi-pencil</v-icon></v-btn>
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
import axios from 'axios'
export default {
  data () {
    return {
        SETTINGS_API: 'http://194.233.68.154:8082/api/settings/',
        SLIDERS_API: 'http://194.233.68.154:8082/api/slider/',
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
        sliderList: [],
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
            text: 'Settings',
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
    getAppSettings(){
        axios({
            method: "get",
            url: this.SETTINGS_API,
            headers: {
                Authorization: this.auth
            }
        })
        .then(r => {
            var settings = r.data.data;
            this.settingsModel.description = settings.description;
            this.settingsModel.shortDescription = settings.shortDescription;
            this.settingsModel.isServerDown = settings.isServerDown;
            this.settingsModel.serverDownMsg = settings.serverDownMsg;
            this.settingsModel.isUnderMaintenance = settings.isUnderMaintenance;
            this.settingsModel.maintenanceMsg = settings.maintenanceMsg;
            this.settingsModel.facebookPage = settings.facebookPage;
            this.settingsModel.youtubeChannel = settings.youtubeChannel;
            this.settingsModel.telegramLink = settings.telegramLink;
            this.settingsModel.imo = settings.imo;
            this.settingsModel.whatsapp = settings.whatsapp;

            this.imageLink = settings.logo


            console.log(this.settingsModel)
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
    uploadLogo(){
        let formData = new FormData();
        formData.append("image", this.imageFile, this.imageFile.name);
        console.log(formData);

        axios({
            method:"POST",
            url: this.SETTINGS_API+'image/',
            data: formData,
            headers: {
                Authorization: this.auth,
                "Content-Type": "multipart/form-data"
            }
        })
        .then(r=>{
            console.log(r);
            this.getAppSettings();
        });
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
      this.getAppSettings()
      this.getSliders()
  }
}
</script>
