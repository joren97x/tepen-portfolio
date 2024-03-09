<script setup>

    // cd2ee0a6d2cd402367433ac1665680c96e1b77b8
    // Bncf83gPdE2
    import {ref, onMounted} from 'vue'
    import axios from 'axios'

    const link = ref('')
    const shortenedLink = ref('')
    const loading = ref(false)
    const token = 'cd2ee0a6d2cd402367433ac1665680c96e1b77b8' //YOUR TOKEN //my token => 7ed9b44598093c5a723c5907d662e54376fedcd0
    const guid = 'Bncf83gPdE2'
    const task = ref('')
    const tasks = ref([])
    const tab = ref(null)

    async function validURL() {
        loading.value = true
        var pattern = new RegExp('^(https?:\\/\\/)?'+ // protocol
            '((([a-z\\d]([a-z\\d-]*[a-z\\d])*)\\.)+[a-z]{2,}|'+ // domain name
            '((\\d{1,3}\\.){3}\\d{1,3}))'+ // OR ip (v4) address
            '(\\:\\d+)?(\\/[-a-z\\d%_.~+]*)*'+ // port and path
            '(\\?[;&a-z\\d%_.~+=-]*)?'+ // query string
            '(\\#[-a-z\\d_]*)?$','i'); // fragment locator
        const valid = !!pattern.test(link.value);
        if(valid) {

            const url = 'https://api-ssl.bitly.com/v4/shorten'
            const data = {
                "group_guid": `${guid}`,
                "domain": "bit.ly",
                "long_url": link.value
            }  
            const config = {
                headers: {
                    Authorization: `Bearer ${token}`,
                    'Content-Type': "application/json"
                }
            }

            await axios.post(url, data, config)
            .then(result => {
                shortenedLink.value = result.data.link
                loading.value = false
            })
            .catch(err => {
                shortenedLink.value = err.message
                loading.value = false
            })
        }
    }

    function deleteTask(id) {
        tasks.value = tasks.value.splice(id, 1)
    }

    function addTask() {
        loading.value = true
        tasks.value.push({
            id: tasks.value.length,
            name: task.value,
            status: false
        })
        task.value = ''
        loading.value = false
    }

    const headers = [
        { title: "Task", align: "center" },
        { title: "Status", align: "center" },
        { title: "Action", align: "center" }
    ]

</script>
<template >
  <v-card>
    <v-tabs v-model="tab" color="deep-purple-accent-4" align-tabs="center">
        <v-tab value="home">Home</v-tab>
        <v-tab value="about">About</v-tab>
        <v-tab value="todo">Todo</v-tab>
        <v-tab value="myapp">Myapp</v-tab>
    </v-tabs>
    <v-window v-model="tab">
        <v-window-item value="home">
                
                

                <section ref="homeRef" class="home">
            <v-card height="500" style="margin-top: 4%;" color="blue-lighten-2">
                <v-row justify="center">
                    <v-col cols="6">
                        <div class="text-center" style="margin-top: 20%">
                            <p class="text-h3" >Hi👋 <br>
                                 I am Stephen Heiward Samson</p>

                        <div class="mt-5">
                            <v-btn icon="mdi-facebook" class="me-2"></v-btn>
                            <v-btn icon="mdi-instagram" class="me-2" src="https://www.instagram.com/alexisjumaoas/"></v-btn>
                            <v-btn icon="mdi-github" src="https://github.com/Alexis/1027" ></v-btn>
                        </div>

                        </div>
                    </v-col>
                    <v-col cols="4">
                        <div style="margin-right: 30%; margin-top:10%" >
                            <v-avatar size="350">
                                <v-img
                                cover
                                src="https://scontent.fceb2-1.fna.fbcdn.net/v/t39.30808-6/323003030_1319207132205733_7857834060582381820_n.jpg?_nc_cat=101&ccb=1-7&_nc_sid=5f2048&_nc_ohc=jBEspCWNcHsAX-n4QQr&_nc_ht=scontent.fceb2-1.fna&oh=00_AfDdIn_7uFbGURX7WVwdzmVtG_1FWFmHHOnRrC-GxNOaVA&oe=65F0BDB1"></v-img>
                            </v-avatar>
                        </div>
                    </v-col>
                
            </v-row>
            </v-card>
      </section>
           
        </v-window-item>
        <v-window-item value="about">

                <section ref="aboutRef">
        <v-card height="500" style="margin-top: 4%;" color="blue-lighten-2">
        
            <v-row>
                    <v-col cols="6">
                        <div style="margin-left: 40%; margin-top:10%">
                            <v-avatar size="300">
                            <v-img cover src="https://scontent.fceb2-1.fna.fbcdn.net/v/t39.30808-6/323003030_1319207132205733_7857834060582381820_n.jpg?_nc_cat=101&ccb=1-7&_nc_sid=5f2048&_nc_ohc=jBEspCWNcHsAX-n4QQr&_nc_ht=scontent.fceb2-1.fna&oh=00_AfDdIn_7uFbGURX7WVwdzmVtG_1FWFmHHOnRrC-GxNOaVA&oe=65F0BDB1"></v-img>
                        </v-avatar>
                        </div>
                    </v-col>
                    <v-col cols="6" style="margin-top: 5%">
                        <p class="text-h4">About me</p><br>
                        <p class="text-h6"> 
                        
                        <p class="mx-5">
                            My name is Stephen Heiward Samson, 23 years old, and I completed my education at Cordova Catholic Cooperative School. Currently, I'm pursuing a Bachelor of Science degree in Information Technology. In my free time, I enjoy basketball and engaging in online gaming."
                            
                        </p>
                         </p>
                    </v-col>
                    
            </v-row>
        </v-card>
      </section>

        </v-window-item>
        <v-window-item value="todo">
            <v-container style="margin-top: 4%;" class="bg-blue-lighten-2">
                
                <v-text-field v-model="task" label="Task" :loading="loading">
                    <template v-slot:append>
                        <v-btn @click="addTask()" :loading="loading">Add task</v-btn>
                    </template>
                </v-text-field>
                <v-data-table :items="tasks" :headers="headers">
                
                        <template v-slot:item="{item}">
                            <tr>
                                <td class="text-center">{{ item.name }}</td>
                                <td class="text-center">{{ item.status }}</td>
                                <td class="justify-center d-flex">
                                    <v-btn :prepend-icon="item.status ? 'mdi-check' : ''" size="small" class="me-3 mt-3" @click="item.status = !item.status" color="green">{{ item.status ? 'Completed' : 'Complete' }}</v-btn>
                                    <v-btn prepend-icon="mdi-delete-empty-outline" size="small" class="mt-3" @click="tasks.splice(item.id, 1)"  color="red">Delete</v-btn>
                                </td>
                            </tr>
                        </template>

                </v-data-table>
            </v-container>
        </v-window-item>
        <v-window-item value="myapp">
            <v-container fluid class="bg-blue-lighten-2">
                <p class="text-h3 text-center">Link shortener </p>
                <v-text-field label="Enter a URL" class="mt-5" prepend-inner-icon="mdi-link" v-model="link" @keydown.enter="validURL()">

                </v-text-field>
                <v-row class="justify-center">
                    <v-btn color="green" @click="validURL()">Shorten</v-btn>
                    
                </v-row>
                <v-container>
                    <h3> {{ shortenedLink }} </h3>
                <v-row>
                    <v-col cols="5">
                        <v-progress-linear
                    color="deep-purple-accent-4"
                    indeterminate
                    rounded
                    v-show="loading"
                    height="3"
                ></v-progress-linear>
                    </v-col>
                </v-row>
                </v-container>
            </v-container>
        </v-window-item>
    </v-window>
  </v-card>
</template>

