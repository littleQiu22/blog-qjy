<template>
  <div class="text-center">
    <v-dialog
      v-model="dialog"
      width="500"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          v-bind="attrs"
          v-on="on"
        >
            <v-icon left>
                mid-eye-outline
            </v-icon>
            点我查看
        </v-btn>
      </template>

      <v-card dark>
        <v-card-title class="text-h5">
            {{description.title}}
        </v-card-title>

        <v-card-text class="text-h6 caption">
            
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            text
            @click="dialog = false"
          >
            I accept
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
  export default {
    prop:{
        description:{
            title:'暂无',
            setting:['暂无'],
            category:['暂无'],
            solutions:['暂无']
        }
    },

    data () {
      return {
        dialog: false,
      }
    },
  }
</script>