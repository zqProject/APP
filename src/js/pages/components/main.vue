<template>
    <div>
        <div class="status-bar" :style="{'height': statusBarHeight}"></div>
    </div>
</template>

<script>
    import utils from "../Utils/util.js";
    export default {
        data() {
            return {
                curHomeBackTriggerTimes: 1,
                maxHomeBackTriggerTimes: 2,
    
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
                const globalEvent = weex.requireModule('globalEvent')
                globalEvent.addEventListener('homeBack', options => {
                    (this.curHomeBackTriggerTimes === this.maxHomeBackTriggerTimes) && this.$router.finish()
                    this.curHomeBackTriggerTimes++
                        var data = new Date();
                    this.$notice.toast({
                        message: data.getTime()
                    })
                    this.$notice.toast({
                        message: `再按一次退出`
                    })
                })
            }
        }
    }
</script>

<style>
    .status-bar {
        background-color: #FED800
    }
</style>
