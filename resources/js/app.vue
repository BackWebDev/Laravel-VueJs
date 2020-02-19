<template>
    <div>
        <navbar :app="this"></navbar>

        <spinner v-if="loading"></spinner>

        <div v-else>
            <router-view :app="this"></router-view>
        </div>
    </div>
</template>

<script>
    import Navbar from './components/navbar';
    import Helper from './utils/helper';
    export default {
        name: 'app',
        components: { Navbar },
        data() {
            return {
                req: axios.create({
                   baseUrl: BASE_URL
                }),
                user: null,
                loading: false,
                helper: Helper,
            }
        },
        mounted() {
            this.init();
        },
        methods: {
            init()
            {
                this.loading = true;

                this.req.get('auth/init').then((response) => {
                    if(response.data.id)
                    {
                        this.user = response.data;
                    }
                    this.loading = false;
                })
            }
        }
    }
</script>

<style>

</style>