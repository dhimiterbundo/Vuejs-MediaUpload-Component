<template>
    <div>
        <div>
            <div class="mediaupload" ref="button">
                <img class="img-fluid img-upload" :src="mediaAvatar"
                @error="mediaAvatar = defaultAvatar">
                <div class="edit-image-hover">
                    <p>{{$t('bikes.edit')}}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Flow from '@flowjs/flow.js';

    export default {
        name: 'MediaUpload',
        props: {
            resource: {
                type: String,
                required: false
            },
            options: {
                type: Object,
                required: false
            }
        },
        data() {
            return {
                defaultOptions: {
                    allowDuplicateUploads: true,
                    attributes: { accept: 'image/*' }
                },
                defaultAvatar: 'assets/default-bike-avatar.svg'
            };
        },
        computed: {
            mediaAvatar: {
                get() {
                    return this.resource;
                },
                set() {
                    return this.resource;
                }
            }
        },
        mounted() {
            const vm = this;
            const attributes = { accept: 'image/*' };
            const flow = new Flow(vm.defaultOptions);
            // flow.assignBrowse(attributes);
            flow.assignBrowse(this.$refs.button, false, true, attributes);

            flow.on('fileAdded', (file, event) => {
                vm.$emit('fileAdded', { file, event });
            });
            flow.on('fileSuccess', (file, message) => {
                vm.$emit('fileSuccess', { file, message });
            });
            flow.on('fileError', (file, message) => {
                vm.$emit('fileError', { file, message });
            });
        },
        methods: {
            defaultImage() {
                this.resource = this.defaultAvatar;
            }
        }
    };
</script>

<style scoped>

</style>
