<template>
    <div>
        <b-container class="steps">
            <b-row class="align-items-center steps__row">
                <b-col
                    :class="{
                        'steps__col--current' : step === 1,
                        'steps__col--active' : step >= 1,
                    }"
                    class="steps__col">
                    <p class="steps__name">
                        Unwrap {{ fromWrapToken.name || '' }} {{ toWrapToken.name || '' }}</p>
                    <p class="steps__number"><span>1</span></p>
                </b-col>
                <b-col
                    :class="{
                        'steps__col--current' : step === 2,
                        'steps__col--active' : step >= 2,
                    }"
                    class="steps__col">
                    <p class="steps__name">Verify Transaction</p>
                    <p class="steps__number"><span>2</span></p>
                </b-col>
                <b-col
                    :class="{
                        'steps__col--current' : step === 3,
                        'steps__col--active' : step >= 3,
                    }"
                    class="steps__col">
                    <p class="steps__name">Receive {{ toWrapToken.name || '' }}</p>
                    <p class="steps__number"><span>3</span></p>
                </b-col>
            </b-row>
        </b-container>
        <div v-if="step === 1">
            <UnWrapStepOne :parent="this"/>
        </div>
        <div v-if="step === 2">
            <UnWrapStepTwo :parent="this"/>
        </div>
        <div v-if="step === 3">
            <UnWrapStepThree :parent="this"/>
        </div>
        <div
            :class="(loading ? 'tomo-loading' : '')"/>
    </div>
</template>

<script>
import UnWrapStepOne from './UnWrapStepOne'
import UnWrapStepTwo from './UnWrapStepTwo'
import UnWrapStepThree from './UnWrapStepThree'
import store from 'store'

export default {
    name: 'App',
    components: {
        UnWrapStepOne,
        UnWrapStepTwo,
        UnWrapStepThree
    },
    data () {
        return {
            step: 1,
            fromWrapToken: {},
            toWrapToken: {},
            receiveAddress: '',
            config: {},
            transactionHash: '',
            loading: false
        }
    },
    async updated () {
    },
    destroyed () {
    },
    created: async function () {
        this.config = store.get('configBridge') || await this.appConfig()
        this.fromWrapToken = this.$route.params.fromWrapToken || {}
        this.toWrapToken = this.$route.params.toWrapToken || {}
        this.receiveAddress = this.$route.params.receiveAddress

        if (!this.$store.state.address ||
            !this.fromWrapToken.name ||
            !this.toWrapToken.name) {
            this.$router.push({
                path: '/wrap'
            })
        }
    },
    methods: {}
}
</script>
