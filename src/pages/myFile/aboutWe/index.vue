<template>
    <div class="wrap">
        <div class="content">
            <div class="img">
                <img src="https://oa.zgchospital.com/img/wechat/logo.png" alt="">
            </div>
            <h3>中关村医院办公</h3>
            <p>Version 1.0.0</p>
        </div>
    </div>
</template>
<script>
export default {
    data(){

    }
}
</script>
<style lang="scss">
    .wrap{
        .content{
            position: absolute;
            left: 50%;
            top: 30%;
            transform: translate(-50%,-50%);
            .img{
                width: 139rpx;
                height: 139rpx;
                margin: auto;
                margin-bottom: 70rpx;
                img{
                    width: 100%;
                    height: 100%;
                    vertical-align: middle;
                }
            }
            h3{
                color: #333333;
                font-size: 32rpx;
                font-weight: bold;
                text-align: center;
            }
            p{
                font-size: 26rpx;
                color: #999999;
                text-align: center;
                margin-top: 10rpx;
            }
        }
    }
</style>