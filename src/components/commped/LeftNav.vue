<template>
    <div class="LeftNav">
        <ul>
            <li>
                <i class="iconfont icon-goumai"></i>
                <p>收银</p>
            </li>
            <li>
                <i class="iconfont icon-hanbao"></i>
                <p>店铺</p>
            </li>
            <li>
                <i class="iconfont icon-wo"></i>
                <p>商品</p>
            </li>
            <li>
                <i class="iconfont icon-gongnengjianyi"></i>
                <p>设置</p>
            </li>
        </ul>
    </div>
    </template>
    
<script>
    export default {
        name: 'LeftNav',
        data(){
            return{
                
            }
        }
    }
</script>

<style lang="stylus">
    .LeftNav
        width 5%
        color 10px
        float left
        color #fff
        height 100%
        overflow hidden
        background-color #1D8ce0
    .LeftNav ul
        list-style none
        padding 0px
    .LeftNav ul li
        list-style: none;
        text-align: center;
        border-bottom:1px solid #20a0ff;
        padding:10px;
    .iconfont
        font-size 24px

</style>
    