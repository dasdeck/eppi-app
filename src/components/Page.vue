<template>

  <div v-if="wizard.currentStep === index">

    <slot name="header" >
      <h1>{{ header }}</h1>
      <div> step {{ index+1 }} of {{ wizard.pages.length }} </div>
    </slot>
    <slot :page="this" :wizard="wizard"></slot>

  </div>

</template>

<script>


export default {

    inject: ['wizard'],
    props: {
        header: {

        },
        canNext: {
          type: Boolean,
          default: null
        }
    },
    computed: {
      index() {
        return this.wizard.$children.indexOf(this);
      },
      isDone() {
        return this.canNext !== false;
      }
    }

}
</script>

