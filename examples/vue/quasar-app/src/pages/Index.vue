<template>
  <q-page class="flex flex-center column">
    <h3 class="row text-center">{{ msg }}</h3>
    <div class="row">
      <q-btn @click="eventToRust()">SEND MSG</q-btn>
    </div>
  </q-page>
</template>

<script>
import { uid } from 'quasar'

function __onTauriInit (cb) {
  if (window.tauri) {
    cb()
  } else {
    window.onTauriInit = cb
  }
}

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'waiting for rust'
    }
  },
  mounted () {
    __onTauriInit(() => {
      window.tauri.listen('reply', res => {
        this.msg = res.payload.msg
      })
    })
  },
  methods: {
    // set up an event listener
    eventToRust () {
      window.tauri.emit('hello', uid())
    }
  }
}
</script>
