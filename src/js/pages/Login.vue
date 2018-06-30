<template>
    <div>
        <div class="status-bar" :style="{'height': statusBarHeight}"></div>
        <wxc-minibar class="status-bar">
            <text slot="left" class="iconfont" @click="minibarLeftButtonClick ">&#xe610;</text>
            
            <text slot="right" class="iconfont" @click="minibarRightButtonClick">&#xe700;</text>
        </wxc-minibar>
        <wxc-searchbar ref="wxc-searchbar" cancel-label = "搜索" always-show-cancel=true @wxcSearchbarCancelClicked="wxcSearchbarCancelClicked" @wxcSearchbarInputReturned="wxcSearchbarInputReturned" @wxcSearchbarInputOnInput="wxcSearchbarInputOnInput" @wxcSearchbarCloseClicked="wxcSearchbarCloseClicked"
            @wxcSearchbarInputOnFocus="wxcSearchbarInputOnFocus" @wxcSearchbarInputOnBlur="wxcSearchbarInputOnBlur"></wxc-searchbar>
            <div class="search">
                <div>
                    <text></text>
                </div>
                <div>
                    <input type="text" placeholder="搜索" class="input" :autofocus=true value="" @change="onchange"/>
                </div>
            </div>
    </div>
</template>

<script>
    import utils from "./Utils/util.js";
    import {
        WxcMinibar,
        WxcSearchbar
    } from 'weex-ui';
    export default {
        components: {
            WxcMinibar,
            WxcSearchbar,
        },
        data() {
            return {
                value: '',
                curHomeBackTriggerTimes: 1,
                maxHomeBackTriggerTimes: 2,
                message: '再按一次返回键就退出',
                statusBarHeight: weex.config.eros.statusBarHeight ? weex.config.eros.statusBarHeight : 40,
            }
        },
        created() {
            utils.initIconFont();
            // 安卓自定义退出 app
            this.androidFinishApp()
        },
        methods: {
            androidFinishApp() {
                var startDate = 0
                const globalEvent = weex.requireModule('globalEvent')
                globalEvent.addEventListener('homeBack', options => {
                    let data = new Date();
                    if (this.curHomeBackTriggerTimes === 2) {
                        var time = data.getTime() - startDate
                        if (time > 5000) {
                            startDate = data.getTime()
                            this.$notice.toast({
                                message: `${this.message}`
                            })
                        } else {
                            this.$router.finish()
                        }
                    } else {
                        this.curHomeBackTriggerTimes++
                            startDate = data.getTime()
                        this.$notice.toast({
                            message: `${this.message}`
                        })
                    }
    
                })
            },
            minibarLeftButtonClick() {
    
            },
            minibarRightButtonClick() {
    
            },
            onchange(){

            }
        }
    
    }
</script>

<style scoped>
    .status-bar {
        background-color: #FED800
    }
    .iconfont {
        font-family: iconfont;
        font-weight: bold;
        font-size: 36;
    }
    .search{
        background-color: #FED800
    }
</style>

    
