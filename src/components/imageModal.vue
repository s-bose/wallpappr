<template>
        <div ref="self" class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLabel">{{title}}</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        ...
                    </div>
                    <div class="modal-footer">
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
            title: "Empty"
        }
    },
    name: 'imageModal',
    methods: {
        close() {
            jquery(this.$refs.self).modal("hide");
        },
        open(title) {
            this.title = title;
            jquery(this.$refs.self).modal("show");
        }
    },
    props: {}
}
</script>

<style scoped>
/* style goes here */
</style>