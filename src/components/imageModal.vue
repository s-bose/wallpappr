<template>
        <div ref="self" class="modal fade bd-example-modal-lg" id="imageModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document">
                <div class="modal-content">
                    <img :src="thumb_src" alt="" height="100%" width="100%">
                    <!-- <div class="modal-body">
                        ...
                    </div> -->
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
            this.thumb_src = post.data.preview.images[0].source.url.replace('amp;', '')
            console.log(this.thumb_src)
            this.title = post.data.author
            jquery(this.$refs.self).modal("show");
        }
    },
    props: {}
}
</script>

<style scoped>
/* .modal-dialog {
    
    width: auto !important;
    display: inline-block;
} */

.modal-content {
    border-radius: 0px !important;
}



.no-border {
    border: none;
}

/* .modal-content {
    backgroundImage: `url(this.post.data.preview.images[0].source.url.replace('amp;s', 's'))`
} */
</style>