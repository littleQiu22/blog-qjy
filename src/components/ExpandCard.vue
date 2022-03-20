<template>
    <div class="expand-card">
        <v-card-title class="py-0">
            <h1 :class="[title_size,text_color]"><slot name="title">默认标题</slot></h1>
            <v-btn
            icon
            @click="show = ! show"
            >
            <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
            </v-btn>
        </v-card-title>

        <v-expand-transition>
            <div v-show="show">
                <v-divider></v-divider> 
                <slot name="body">默认正文</slot>
            </div>
        </v-expand-transition>
    </div>
</template>

<script>
export default {
    props:{
        title_size:'text-h5',
        text_color:'blue--text',
        initial_show:false
    },
    data(){
        return{
            show:(this.initial_show?true:false)
        }
       
    }
}
</script>

<style>

</style>
