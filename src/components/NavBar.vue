<template>
    <nav class="chinese">
        <!-- <v-main> -->
            <v-app-bar flat app  clipped-left dark>         
                <v-app-bar-nav-icon class="white--text" @click="drawer=!drawer"></v-app-bar-nav-icon>
                <v-app-bar-title class="text-uppercase blue--text ">
                    <span class="font-weight-light">easy or</span>
                </v-app-bar-title>
                <v-spacer></v-spacer>
                <v-btn depressed outlined color="grey">
                    <span>sign out</span>
                    <v-icon right>mdi-exit-to-app</v-icon>
                </v-btn>   
            </v-app-bar>
        <!-- </v-main> -->

        <v-navigation-drawer app clipped v-model="drawer" dark>
            <v-layout column align-center>
                <v-flex>
                    <v-avatar size="100">
                    <v-img :src="images.profile" alt="logo" height="100px" width="100px"></v-img>
                    </v-avatar>
                </v-flex>              
            </v-layout>
                         
            <v-list >
                <v-expansion-panels  multiple  :value="panel" >
                    <v-list-item  v-for="link in links" :key="link.text" router :to="link.route">
                            <v-expansion-panel>
                                <v-expansion-panel-header  color="#363636" class="py-0">
                                    <v-list-item-action>
                                        <v-icon class="white--text">{{link.icon}}</v-icon>
                                    </v-list-item-action>
                                    {{link.text}}
                                </v-expansion-panel-header>
                                <v-expansion-panel-content color="#363636">
                                    <v-list-item v-for="(item,i) in link.subtext" :key="i" router :to="link.subroute[i]" color="#363636" class="px-0">
                                        <div >
                                            <v-icon class="white--text">{{link.subicon[i]}}</v-icon>
                                        </div>
                                        <v-spacer></v-spacer>
                                        <div>
                                            <span>{{link.subtext[i]}}</span>
                                        </div>
                                        
                                    </v-list-item>                             
                                </v-expansion-panel-content>
                            </v-expansion-panel>
                    </v-list-item>
                </v-expansion-panels>
            </v-list>
        </v-navigation-drawer>
    </nav>
</template>

<script>
export default {
    data(){
        return{
            panel:[1],
            drawer:false,
            links:[
                {icon:'mdi-information-outline',text:'关于本站',route:'/about'},
                {icon:'mdi-package-variant-closed',text:'运筹专栏',subtext:["车辆路径规划","列生成框架"],subicon:["mdi-rv-truck","mdi-family-tree"],subroute:["/api/vrp","/api/cg"]}
            ],
            images:{
                profile:require('../assets/logo.png')
            }
        }
    }
    
}
</script>

