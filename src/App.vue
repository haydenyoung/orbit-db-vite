<template>
  <v-app>
    <v-main>
      <profile-editor/>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import ProfileEditor from './components/ProfileEditor.vue'

export default defineComponent({
  name: 'App',

  components: {
    ProfileEditor,
  },

  data () {
    return {
      //
    }
  },
})
</script>
