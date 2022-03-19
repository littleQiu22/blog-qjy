<template>
<div>
    <v-card>
        <v-card-title>
        <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="输入api名称进行搜索"
            single-line
            hide-details
        ></v-text-field>
        </v-card-title>
        <v-data-table
            :headers="headers"
            :items="assets"
            :search="search"
            :sort-by="['status','stars']"
            :sort-desc="[false,false]"
            multi-sort
            :loading="loading"
            loading-text="正在加载..."
            >

            <template v-slot:[`item.api`]="{ item }">
                <div style="width:90px">
                    {{item.api}}
                </div> 
            </template>

            <template v-slot:[`item.description`]="{ item }">
                <div style="width:80px">
                    <IntroCard :description="item.description"/>
                </div>
            </template>

            <template v-slot:[`item.status`]="{ item }">
                <v-btn :color="getStatusColor(item.status)">
                    {{getStatus(item.status)}}
                </v-btn>
            </template>

            <template v-slot:[`item.stars`]="{ item }">
                <div style="width: 100px">
                    <v-rating
                    length="5"
                    size="10"
                    dense
                    :value="item.stars"
                    ></v-rating>
                </div>
                
            </template>

            <template v-slot:[`item.link`]="{ item }">
                <div v-if="online(item.status)">
                    <router-link :to="item.link">
                        <v-btn>
                            <v-icon left>mdi-gesture-tap</v-icon>
                            <span>点我跳转</span>
                        </v-btn> 
                    </router-link>
                </div>
                <div v-else>
                    <router-link :to="item.link">
                        <v-btn>
                            <v-icon left>mdi-email-fast</v-icon>
                            <span>给我建议</span>
                        </v-btn> 
                    </router-link>
                </div>
            </template>
        </v-data-table>
    </v-card>   
</div>
</template>

<script>
import IntroCard from '../components/IntroCard.vue'

  export default {
    components:{
        IntroCard
    },
    props: [
        "headers","assets"
    ],
      
    data(){
        return {
            search:"",
            loading:true
        }
    },

    methods: {
        getStatus(status){
            this.loading=false;
            if(typeof(status)!='string' || status.length==0){
                return 'unknown';
            }
            let char=status.charAt(0);
            if(char=='0'){
                return '上线于'+status.substring(2,status.length);
            }
            if(char=='1'){
                return '即将上线';
            }
            if(char=='2'){
                return '等待开发';
            }
            return 'unknown';
        },

        getStatusColor(status){
            if(typeof(status)!='string' || status.length==0){
                return 'tomato';
            }
            let char=status.charAt(0);
            if(char=='0'){
                return 'blue';
            }
            if(char=='1'){
                return 'orange';
            }
            if(char=='2'){
                return 'red';
            }
            return 'tomato';
        },

        online(status){
            if(typeof(status)!='string' || status.length==0){
                return false;
            }
            let char=status.charAt(0);
            if(char=='0'){
                return true;
            }
            return false;
        }
    },
  }
</script>