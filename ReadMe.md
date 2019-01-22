# Vuejs Component for Media Upload.

For this media component i have used Flowjs library.

This component previews the image you upload and on click it allows you to upload a new image.

This is a single upload component .
 
 ``` javascript
 
  import Flow from '@flowjs/flow.js'; // import flow.js
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
                }, // these are default options for component , can add more as we may need 
                defaultAvatar: 'assets/default-bike-avatar.svg' //choose one image as default if there is no image given as prop
            };
        },
        computed: { // on computed we have TWO main METHODS  GET() and SET()
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
            }); // event fired when a file was added
            flow.on('fileSuccess', (file, message) => {
                vm.$emit('fileSuccess', { file, message });
            }); // event fired when file added successfully
            flow.on('fileError', (file, message) => {
                vm.$emit('fileError', { file, message });
            }); // event fired when upload was not successfully
        },
        methods: {
            defaultImage() {
                this.resource = this.defaultAvatar;
            }
        }
    };
