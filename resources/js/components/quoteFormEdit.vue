<template>
    <div>
        <div class="row mt-4">
            <div class="col-12">
                <div class="breadcrumb-main">
                    <h4 class="text-capitalize breadcrumb-title">Quote update</h4>
                </div>
            </div>
        </div>
        <div class="row">
            <div class="col-lg-8">
                <div class="card card-Vertical card-default card-md mb-4">
                    <div class="card-body pb-md-30">
                        <div class="Vertical-form">
                            <div class="form-group">
                                <label for="">Quote Indonesia</label>
                                <textarea v-model="quotes.quoteid" class="form-control"
                                    placeholder="Requirements / Decriptions / Qualification / Job descriptions. Press `CTRL + ENTER` to send the comment."
                                    id="" cols="30" rows="10"></textarea>
                            </div>
                            <div class="form-group">
                                <label for="">Quote Inggris</label>
                                <textarea v-model="quotes.quoteen" class="form-control"
                                    placeholder="Requirements / Decriptions / Qualification / Job descriptions. Press `CTRL + ENTER` to send the comment."
                                    id="" cols="30" rows="10"></textarea>
                            </div>
                            <div class="form-group">
                                <div class="form-row justify-content-end">
                                    <div class="layout-button mt-25">
                                        <button v-on:click="handleSubmit" type="submit"
                                            class="btn btn-success btn-default btn-squared px-30">update
                                            quote</button>
                                        <button v-on:click="doneQuote" type="submit"
                                            class="btn btn-primary btn-default btn-squared px-30"
                                            v-if="this.user.role == 'it'"><i class="fas fa-check"></i>&nbsp;
                                            Done</button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    import Swal from 'sweetalert2';

    export default {
        title() {
            return 'Quote update';
        },
        data() {
            return {
                quotes: {},
                user: {},
            }
        },
        created() {
            this.loadUserLoggedIn();
            this.loadDataQuote();
        },
        methods: {
            async loadUserLoggedIn() {
                const resp = await axios.get('/getUserLoggedIn');
                this.user = resp.data;
            },
            async loadDataQuote() {
                const resp = await axios.get('/api/quote/' + this.$route.params.id);
                this.quotes = resp.data[0];
            },
            async handleSubmit() {
                await axios.patch('/api/quote/' + this.$route.params.id, this.quotes);
                Swal.fire({
                    icon: 'success',
                    title: 'Congratulations',
                    text: 'Success update job vacancy.',
                });
                this.$router.push('/quote');
            },
            async doneQuote() {
                await axios.patch('/api/quote/approved/' + this.$route.params.id);
                this.$router.push('/quote');
                Swal.fire({
                    icon: 'success',
                    title: 'Congratulations',
                    text: 'Success approve this quote.',
                });
            }
        },
    }

</script>
