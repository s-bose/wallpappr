<template>
        <div ref="self" class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header no-border">
                        <h5 class="modal-title" id="exampleModalLabel">{{title}}</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        ...
                    </div>
                    <div class="modal-footer no-border">
                        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                        <button type="button" class="btn btn-primary">Save changes</button>
                    </div>
                </div>
            </div>
        </div>
</template>

<script>

const jquery = require('jquery')

export default {
    data() {
        return {
            thumb_src: String,
            title: String,
            author: String,
            // required: true,
            
        }
    },
    name: 'imageModal',
    methods: {
        close() {
            jquery(this.$refs.self).modal("hide");
        },
        open(post) {
            this.title = post.data.author
            jquery(this.$refs.self).modal("show");
        }
    },
    props: {}
}
</script>

<style scoped>

.modal-content {
    border-radius: 0px !important;
}

.no-border {
    border: none;
}

</style>