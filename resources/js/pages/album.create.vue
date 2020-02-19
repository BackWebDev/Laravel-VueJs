<template>
    <div class="container">
        <div class="card">
            <div class="card-header">
                Create Album
            </div>
            <div class="card-body">
                <div class="col-md-8 offset-md-2">
                    <form v-on:submit.prevent="onSubmit" style="margin: 20px">
                        <div class="form-group">
                            <label>Title</label>
                            <input type="text" v-model="title" placeholder="Title" class="form-control" v-bind:class="{ 'is-invalid': errorTitle }">
                            <div class="invalid-feedback">
                                {{ errorTitle }}
                            </div>
                            <div class="form-group">
                                <label>Description</label>
                                <input type="text" placeholder="Description" v-model="desc" class="form-control">
                            </div>

                            <button type="submit" class="btn btn-lg btn-primary">Go!</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'album-create',
        props: ['app'],
        data() {
            return {
                title: '',
                errorTitle: null,
                desc: '',
                loading: false,
            }
        },
        mounted() {
            if(this.app.user === null)
            {
                this.$router.push({
                    name: 'auth.login'
                })
            }
        },
        methods: {
            onSubmit()
            {
                if(!this.title)
                {
                    this.errorTitle = 'Title is required.';
                    return null;
                }

                let data = {
                    title: this.title,
                    desc: this.desc
                }

                this.loading = true;

                this.app.req.post('album', data).then(response => {
                    this.loading = false;
                    this.$router.push({
                        name: 'album.show',
                        params: {
                            id: response.data.id
                        }

                    })
                })
            }
        }
    }
</script>

<style>

</style>