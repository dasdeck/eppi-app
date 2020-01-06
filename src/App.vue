<template>

  <div id="app">
    <Wizard @done="sendEntity" v-slot="{wizard}" >

        <Page header="Name" :canNext="entity.name !== ''">
          <form @submit="$event.preventDefault() || wizard.next()">
            <input type="text" v-model="entity.name"/>
          </form>
        </Page>

        <Page header="Summary">
          you have choosen "{{entity.name}}" as name
        </Page>

    </Wizard>
  </div>

</template>

<script>
import Wizard from './components/Wizard.vue'
import Page from './components/Page.vue'

export default {
  name: 'app',
  data() {
    return {entity: {name: ''}};
  },
  components: {
    Wizard,
    Page
  },
  methods: {
    sendEntity() {
      debugger
    }
  }
}
</script>