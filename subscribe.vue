<template>
    <div>
        <section>
            <b-row>
                <b-col cols="2" class="side-menu">
                <!-- <SideProfile /> -->
                </b-col>

                <b-col cols="10">
                    <div id="component-container"></div>
                </b-col>
            </b-row>
        </section>
    </div>
</template>

<script>
export default {
    name: 'profile-subscription2',

    head: {
        titleTemplate: "Profile Subscription2 | %s",
        meta: [
            { hid: "description", name: "description", content: "Profile Subscription2 Page" }
        ],
        link: [
        { rel: "stylesheet", type: "text/css", href: "https://checkoutshopper-test.adyen.com/checkoutshopper/sdk/3.10.1/adyen.css" }
        ],
        script: [
        { src: "https://checkoutshopper-test.adyen.com/checkoutshopper/sdk/3.10.1/adyen.js" }
        ],
    },

    mounted() {

    }
}
</script>
