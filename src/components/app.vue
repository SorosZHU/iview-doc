<style scoped>
    @import "../styles/common.css";
    .icon-loading{
        position: fixed;
        top: 10px;
        right: 10px;
        color: #0099e5;
        animation: ani-app-loading 1s linear infinite;
    }
    @keyframes ani-app-loading {
        from { transform: rotate(0deg);}
        50%  { transform: rotate(180deg);}
        to   { transform: rotate(360deg);}
    }
</style>
<template>
    <div>
        <router-view></router-view>
        <Back-top :bottom="100"></Back-top>
        <transition name="fade">
            <div class="loading" v-show="loading">
                <Icon type="load-c" size="18" class="icon-loading"></Icon>
            </div>
        </transition>
    </div>
</template>
<script>
    import bus from './bus';

    export default {
        data () {
            return {
                liveVisible: false,
                iViewVisible: false
            }
        },
        computed: {
            loading () {
                return bus.loading;
            }
        },
        mounted () {
            if (window.localStorage.getItem('liveModalTime')) {
                const time = parseInt(window.localStorage.getItem('liveModalTime'));
                const today = this.getTodayUnix();
                if ((today - time) > 86400000 * 7) this.liveVisible = true;
            } else {
                this.liveVisible = true;
            }

            if (this.liveVisible) {
                this.$Notice.config({
                    top: 85
                });
                this.$Notice.info({
                    title: 'Recruiting Translation Volunteer',
                    desc: 'iView team are recruting volunteers to help us translate the document. If you master both Chinese and English, we are looking forward to your joining in our translation plan and help us improve iView. If you want to join in the translation plan, please send E-Mail to <a href="mailto:admin@aresn.com">admin@aresn.com</a>',
                    duration: 0,
                    onClose: () => {
                        const today = this.getTodayUnix();
                        window.localStorage.setItem('liveModalTime', today);
                        this.$Message.success('关闭成功，一周内不再提示', 5);
                    }
                });
                this.$Notice.config({
                    top: 24
                });
            }
        },
        methods: {
            getTodayUnix () {
                const date = new Date();
                date.setHours(0);
                date.setMinutes(0);
                date.setSeconds(0);
                date.setMilliseconds(0);
                return date.getTime();
            },
            handleLive () {
                const today = this.getTodayUnix();
                window.localStorage.setItem('liveModalTime', today);
            }
        }
    }
</script>
