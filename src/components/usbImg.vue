<template>
    <div class="wrap">
        <img v-if="fileExtension=='jpg'||fileExtension=='png'" :src="link" alt="">
        <img v-if="fileExtension=='rar'" src="https://oa.zgchospital.com/img/wechat/rar.png" alt="">
        <img v-if="fileExtension=='txt'" src="https://oa.zgchospital.com/img/wechat/02.3.1.Txt.png" alt="">
        <img v-if="fileExtension=='pdf'" src="https://oa.zgchospital.com/img/wechat/02.3.1.Pdf.png" alt="">
        <img v-if="fileExtension=='ppt'" src="https://oa.zgchospital.com/img/wechat/02.3.1.PPT.png" alt="">
        <img v-if="fileExtension=='word'" src="https://oa.zgchospital.com/img/wechat/word.png" alt="">
    </div>
</template>
<script>
export default {
    name:"UsbImg",
    props:['fileExtension','link'],
    data(){
        return {

        }
    }
}
</script>