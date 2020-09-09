<!-- 会话记录 -->
<template>
    <div class="imRecord-wrapper">
        <header class="header">
            <div class="kf-info-wrapper">
                <img class="kf-avatar" :src="storeServerChatEn.avatarUrl" />
                <div class="kf-content">
                    <span class="kf-name">{{storeServerChatEn.serverChatName}}</span>
                    <p class="kf-status"><span>在线</span><img class="icon-status" src="@@/image/online.png" alt=""></p>
                </div>

            </div>
            <!--<div class="client-info-wrapper">-->
                <!--<p>-->
                    <!--<i class="fa fa-user on-line"></i>{{storeCurrentChatEnlist.length}}</p>-->
            <!--</div>-->
        </header>
        <main class="main">
            <div class="main-title">消息列表</div>
            <div v-if="storeCurrentChatEnlist.length>0" class="item-list">

                <div class="item" v-for="(tmpEn, index) in storeCurrentChatEnlist" :key="index" @click="selectChat(tmpEn)" v-bind:class="{ active: selectedChatEn!=null && tmpEn.clientChatId==selectedChatEn.clientChatId}">
                    <!--<div class="followicon-wrapper">-->
                        <!--<i class="iconfont icon-zhidingwujiaoxing position-h-v-mid" :class="{ active: tmpEn.isFollow}" @click.stop="toggleFollowIcon(tmpEn)"></i>-->
                    <!--</div>-->
                    <!-- 客户端头像 -->
                    <div class="platicon-wrapper">
                        <div class="header-img"></div>
                    </div>
                    <div class="info-wrapper">
                        <p class="first-p">
                            <span class="name">{{tmpEn.clientChatName}}</span>
                            <span class="lastMsgTime">{{getLastMsgTimeStr(tmpEn.lastMsgTime)}}</span>
                        </p>
                        <p class="second-p">
                            <span class="lastMsgContent" v-html="tmpEn.lastMsgContent"></span>
                            <el-badge class="newMsgCount" :value="tmpEn.newMsgCount" :max="99" v-show="tmpEn.newMsgCount>0"></el-badge>
                        </p>
                    </div>
                </div>
            </div>
            <div v-else-if="storeCurrentChatEnlist.length==0" class="empty-wrapper">
                <div class="content">
                    <i class="iconfont fa fa-commenting-o"></i>
                    <p class="title">当前没有会话</p>
                </div>
            </div>
        </main>
    </div>
</template>

<script>
export default {
    data() {
        return {};
    },
    computed: {
        selectedChatEn() {
            return this.$store.imServerStore.getters.selectedChatEn;
        },
        // 当前会话列表
        storeCurrentChatEnlist() {
            return this.$store.imServerStore.getters.currentChatEnlist;
        },
        storeServerChatEn() {
            return this.$store.imServerStore.getters.serverChatEn;
        }
    },
    watch: {},
    methods: {
        /**
         * 选中当前列表的chat
         * @param {Object} en call实体类
         */
        selectChat: function(en) {
            this.$store.imServerStore.dispatch('selectChat', { clientChatId: en.clientChatId });
            this.$emit('selectedChat', {}); // 事件上传
        },

        /**
         * 设置关注
         */
        toggleFollowIcon: function(en) {
            en.isFollow = !en.isFollow;
            // 排序
            this.$store.imServerStore.commit('sortCurrentChatEnlist', {});
        },

        /**
         * 获取背景class
         * @param {string} clientChatName 姓名
         */
        getBgClass: function(clientChatName) {
            var rs = clientChatName.charCodeAt(0) % 5;
            return 'bg' + rs;
        },

        /**
         * 返回chat对象的最后一次消息时间
         * @param {String|Date} sValue 传入的时间字符串
         */
        getLastMsgTimeStr: function(sValue) {
            if (sValue == null) {
                return '';
            }
            var rs = this.$ak.Utils.getDateTimeStr(sValue, 'H:i:s');
            return rs;
        }
    }
};
</script>

<style lang="less">
.imRecord-wrapper {
    width: 100%;
    height: 550px;
    overflow: hidden;
    border: 0;
    & > .header {
        .kf-info-wrapper {
            width: 100%;
            height: 80px;
            padding: 0 20px;
            display: flex;
            align-items: center;
            color: #181818;
            box-sizing: border-box;
            .kf-avatar {
                width: 50px;
                height: 50px;
            }
            .kf-content {
                padding-left: 14px;
            }
            .kf-name {
                font-size: 16px;
            }
            .kf-status {
                color: #747474;
                font-size: 14px;
                display: flex;
                align-items: center;
                padding-top: 6px;
                .icon-status {
                    width: 14px;
                    margin-left: 10px;
                }
            }
        }
        .client-info-wrapper {
            p:first-child {
                margin-bottom: 5px;
            }
            .fa {
                margin-right: 10px;
            }
        }
    }
    & > .main {
        height: calc(~'100% - 80px');
        position: relative;
        background: #fafafa;
        .main-title {
            background: #ECECEC;
            line-height: 30px;
            padding-left: 16px;
            box-sizing: border-box;
            font-size: 14px;
            color: #181818;
        }
        .item-list {
            height: calc(~'100% - 46px');
            overflow-y: auto;
            .item {
                position: relative;
                padding: 12px;
                border-bottom: 1px solid #e6e6e6;
                cursor: pointer;
                display: flex;
                align-items: center;
                &.active,
                &:hover {
                    background-color: #D6D6D6;
                    /*.info-wrapper .first-p .name,*/
                    /*.info-wrapper .second-p .lastMsgContent,*/
                    /*.info-wrapper .first-p .lastMsgTime {*/
                        /*color: #eaf4fb;*/
                    /*}*/
                    .iconfont {
                        display: initial !important;
                    }
                }
                /*.followicon-wrapper {*/
                    /*width: 25px;*/
                    /*height: 100%;*/
                    /*.iconfont {*/
                        /*display: none;*/
                        /*font-size: 12px;*/
                        /*color: #eaf4fb;*/
                        /*&.active {*/
                            /*display: initial;*/
                            /*color: #f9ce1d;*/
                        /*}*/
                    /*}*/
                /*}*/
                .platicon-wrapper {
                    width: 50px;
                    height: 100%;
                    .header-img {
                        width: 50px;
                        height: 50px;
                        border-radius: 50%;
                        background: url("../../../static/image/default-avatar.png");
                        background-size: 100% 100%;
                    }
                }
                .info-wrapper {
                    position: relative;
                    float: left;
                    width: 185px;
                    padding-left: 14px;
                    .first-p {
                        overflow: hidden;
                        padding-top: 11px;
                        .name {
                            display: inline-block;
                            float: left;
                            width: 50%;
                            font-size: 14px;
                            color: #181818;
                            white-space: nowrap;
                            text-overflow: ellipsis;
                            overflow: hidden;
                            text-align: left;
                        }
                        .lastMsgTime {
                            float: right;
                            font-size: 12px;
                            color: #747474;
                        }
                    }
                    .second-p {
                        clear: both;
                        padding-top: 5px;
                        line-height: 1.2;
                        .lastMsgContent {
                            display: inline-block;
                            width: 150px;
                            margin-top: 3px;
                            font-size: 12px;
                            color: #8d8d8d;
                            white-space: nowrap;
                            text-overflow: ellipsis;
                            text-align: left;
                            overflow: hidden;
                        }
                        .newMsgCount {
                            position: relative;
                            top: -3px;
                            float: right;
                            .el-badge__content {
                                border: 1px solid #ffffff00;
                                background: #FE1530;

                            }
                        }
                    }
                }
            }
        }
        .empty-wrapper {
            font-size: 16px;
            color: #3e3e3e;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            .content {
                width: 170px;
                height: 200px;
                margin: 0px auto;
                text-align: center;
                color: #867c7c;
                .iconfont {
                    font-size: 90px;
                }
                .title {
                    margin-top: 25px;
                }
            }
        }
    }
}
</style>
