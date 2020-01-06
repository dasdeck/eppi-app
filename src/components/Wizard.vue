<template>

  <div>

    <slot :wizard="this"></slot>

    <!-- we can skip rendering the buttons on first render as we need to evaluate the rendererd pages first -->
    <slot name="footer">

        <template v-if="currentStep >= 0">
            <component v-if="prevButtonComponent" :is="prevButtonComponent"/>
            <button v-else-if="prevButtonComponent !== false && !first" @click="prev()">prev</button>

            <component v-if="nextButtonComponent" :is="nextButtonComponent"/>
            <template v-else-if="nextButtonComponent !== false" >
                <button :disabled="!canNext" v-if="!last"  @click="next()">next</button>
                <button :disabled="!isDone" v-else  @click="$emit('done')">done</button>
            </template>
        </template>

    </slot>

  </div>

</template>

<script>
/**
 since the component's logic relies on the slot content, most evaluation can only take place on/after mount
*/
export default {

    components: {
        Vnode: {
            functional: true,
            render: (h, ctx) => ctx.props.vnode
        }
    },

    provide() {
        return {wizard: this};
    },

    data: () => ({currentStep: -1}),

    mounted() {
        // set current step on mount to fore re.evaluation of current step
        this.currentStep = 0;
    },

    computed: {
        prevButtonComponent() {
            return null;
        },
        nextButtonComponent() {
            return null;
        },
        first() {
            return this.currentStep === 0;
        },
        last() {
            return this.currentStep === this.pages.length -1;
        },
        pages() {
            return this.$children.filter(comp => comp)
        },
        isDone() {
            return this.pages.reduce((car, val) => car && val.isDone !== false, true);
        },
        currentPage() {
            return this.pages[this.currentStep];
        },
        canNext() {
            return this.currentPage && this.currentPage.canNext !== false;
        }
    },
    methods: {

        prev() {
            if (!this.first) this.currentStep--;
        },
        next()
        {
            if (!this.last) this.currentStep++;
        }
    }
}
</script>

