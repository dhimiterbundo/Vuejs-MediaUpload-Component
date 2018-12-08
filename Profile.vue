<template>
    <div>
           <media-upload
                        v-on:fileAdded="fileAdded"
                        :resource="profile.avatar.avatar.url">
        </media-upload>
    </div>
</template>
<script>
    import MediaUpload from '@/MediaUpload';
    import FileService from '@/FileService';

    export default {
           components: {
            MediaUpload
           },
            methods: {
            fileAdded(data) {
                const vm = this;
                if (data.file.size > 10 * 1024 * 1024) {
                    notificationService.warning(vm.$t('common.too_large'));
                } else {
                    FileService.getBase64(data.file.file).then((encodedImage) => {
                        vm.uploadImage(encodedImage);
                    });
                }
            },
            uploadImage(encodedImage) {
                const vm = this;
                const image = encodedImage.split(',')[1];
                const payload = {
                    avatar: image,
                };
                vm.showSpinner = true;
                vm.$store.dispatch('profile/addProfileAvatar', payload).then(() => {
                    notificationService.success(vm.$t('profile.image_updated_successfully'));
                    vm.showSpinner = false;
                }, (error) => {
                    notificationService.error(error);
                    vm.showSpinner = false;
                });
            },
        },
    }