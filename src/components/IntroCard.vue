<template>
  <div class="text-center">
    <v-dialog
      v-model="dialog"
      width="500"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color=" lighten-2"
          dark
          v-bind="attrs"
          v-on="on"
        >
          点我查看
        </v-btn>
      </template>

      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
            {{description.title}}
        </v-card-title>


        <v-list>
            <v-list-group
                :value="true"
                color="black"
                prepend-icon="mdi-tag-multiple-outline"
                v-for="key in description.keys"
                :key="key"
            >
                <template v-slot:activator>
                    <v-list-item-title class="text-h6">{{description.translate[key]}}</v-list-item-title>
                </template>
                <v-list-item
                    v-for="script in description[key]"
                    :key="script"
                >
                 <v-divider
                    class="mx-4"
                    :inset="inset"
                    vertical
                ></v-divider>
           
                    <p style="text-indent:1em">
                        <v-icon>mdi-dots-horizontal-circle-outline</v-icon>
                        {{script}}
                    </p> 
                </v-list-item>
            </v-list-group>

            <v-list-group
                :value="true"
                color="black"
                prepend-icon="mdi-tag-multiple-outline"
            >
                <template v-slot:activator>
                    <v-list-item-title class="text-h6">参考链接</v-list-item-title>
                </template>
                <v-list-item>
                    <v-divider
                        class="mx-4"
                        :inset="inset"
                        vertical
                    ></v-divider>
           
                    <p style="text-indent:1em">
                        <v-icon>mdi-dots-horizontal-circle-outline</v-icon>
                        <span v-for="(item,key) in description.references" :key="key">
                            <a :href="item" style="text-decoration:none">{{key}}</a>
                            <span>; </span>

                        </span>
                    </p> 
                </v-list-item>
            </v-list-group>
        </v-list>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="grey lighten-2"
            @click="dialog = false"
          >
            关闭
          </v-btn>
          <v-spacer></v-spacer>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  props:[
        "description"
    ],

    data () {
      return {
        dialog: false,
      }
    }, 
  }

</script>