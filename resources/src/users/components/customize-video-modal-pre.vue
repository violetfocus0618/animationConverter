<template>
    <b-modal hide-header id="customize-video-modal" ref="modal-ref" size="xl" title="Customize Video Template" hide-footer
             @hidden="onHidden"
    >
        <div class="modal--content" v-if="videoTemplate">
            <div class="row">
                <div class="col-md-auto pr-md-0">
                    <div class="video-player">
                        <template-player
                            :template="videoTemplate.data_json"
                            :controlState="playerState"
                            :control-seek="playerSeek"
                            @onStop="onStop"
                            ref="templatePlayer"
                        ></template-player>
                    </div>
                </div>
                <div class="col-md pl-md-0">
                    <div class="video-content">
                        <div class="title-wrap">
                            <h2>{{videoTemplate.temp_name}}</h2>
                            <div><i class="fa fa-burn"></i> {{videoTemplate.aspect_type}}</div>
                        </div>
                        <div class="description-wrap">
                            <h2>Description</h2>
                            <div>
                                {{videoTemplate.temp_description}}
                            </div>
                        </div>
                        <div class="start-customize-wrap">
                            <a v-bind:href="'/edit-video?id=' + videoTemplate.id" class="btn btn-block text-center">Start Customize This Video</a>
                        </div>
                    </div>
                </div>
            </div>
            <div class="video-aside-wrap">
                <div class="row">
                    <div class="col-md-3 col-sm-6">
                        <div class="aside-item duration-wrap">
                            <span class="item-name">DURATION</span>&nbsp;&nbsp;
                            <span class="item-value">{{formatDuration(videoTemplate.duration)}}</span>
                        </div>
                    </div>
                    <div class="col-md-3 col-sm-6">
                        <div class="aside-item you-need-wrap">
                            <span class="item-name">YOU'LL NEED</span>
                            <i class="fa fa-image"></i> <span class="item-value">4</span>
                            <i class="fa fa-camera"></i> <span class="item-value">0</span>
                        </div>
                    </div>
                    <div class="col-md-3 col-sm-6">
                        <div class="aside-item ratio-wrap">
                            <span class="item-name">Video Ratio</span>
                            <i class="fa fa-react"></i>
                            <span class="full-hd">Full HD</span>
                        </div>
                    </div>
                    <div class="col-md-3 col-sm-6">
                        <div class="aside-item color-palette-wrap">
                            <span class="item-name">Color palette</span> :
                            <span class="item-value">1</span>
                        </div>
                    </div>
                </div>
            </div>
            <div class="video-variations">
                <div class="section-name">Variations</div>
                <div class="list-item">
                    <div class="variation-item">
                        <div class="item-thumb">
                            <img src="storage/libraries/backgrounds/wide_01.jpg" alt="" />
                        </div>
                        <div class="item-content">
                            <div class="item-name">Square</div>
                            <div class="item-values">
                                <div class="row">
                                    <div class="col-auto pr-0">
                                        <div class="item-value">0:14</div>
                                    </div>
                                    <div class="col-auto mr-auto">
                                        <div class="item-value">
                                            <i class="fa fa-react"></i>
                                            <span>1:1</span>
                                        </div>
                                    </div>
                                    <div class="col-auto pr-0">
                                        <div class="item-value">
                                            <i class="fa fa-image"></i> <span>4</span>
                                        </div>
                                    </div>
                                    <div class="col-auto">
                                        <div class="item-value">
                                            <i class="fa fa-camera"></i> <span>0</span>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="variation-item type-vertical">
                        <div class="item-thumb">
                            <img src="storage/libraries/backgrounds/wide_01.jpg" alt="" />
                        </div>
                        <div class="item-content">
                            <div class="item-name">Vertical</div>
                            <div class="item-values">
                                <div class="row">
                                    <div class="col-auto">
                                        <div class="item-value">0:14</div>
                                    </div>
                                    <div class="col-auto mr-auto">
                                        <div class="item-value">
                                            <i class="fa fa-react"></i>
                                            <span>1:1</span>
                                        </div>
                                    </div>
                                    <div class="col-auto">
                                        <div class="item-value">
                                            <i class="fa fa-image"></i> <span>4</span>
                                        </div>
                                    </div>
                                    <div class="col-auto">
                                        <div class="item-value">
                                            <i class="fa fa-camera"></i> <span>0</span>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="color-palette">
                <div class="section-name">Color Palette</div>
                <div class="list-item">
                    <div class="style-item">
                        <div class="input-wrap d-inline-block">
                            <input id="video-style-1" type="radio" name="video_style" checked />
                            <label for="video-style-1"></label>
                        </div>
                        Style 01
                    </div>
                    <div class="style-item">
                        <div class="input-wrap d-inline-block">
                            <input id="video-style-2" type="radio" name="video_style" />
                            <label for="video-style-2"></label>
                        </div>
                        Style 02
                    </div>
                    <div class="style-item">
                        <div class="input-wrap d-inline-block">
                            <input id="video-style-3" type="radio" name="video_style" />
                            <label for="video-style-3"></label>
                        </div>
                        Style 03
                    </div>
                </div>
            </div>
        </div>
    </b-modal>
</template>

<script>
    import BModal from 'bootstrap-vue/es/components/modal/modal';
    import TemplatePlayer from './../components/players/template-player/template-player.vue';
    import AppHelper from './../../app-helper';
    export default {
        name: "customize-video-modal",
        props: {
            videoTemplate: {
                type: [Object, Boolean],
                default: false
            }
        },
        components: {
            'b-modal': BModal,
            TemplatePlayer
        },
        data: function() {
            return {
                playerState: 'play',
                playerSeek: 0,
            }
        },
        methods: {
            open: function () {
                this.$refs['modal-ref'].show();
            },
            onStop() {
                this.playerState = 'pause';
                this.playerSeek = 3000;
                this.$refs.templatePlayer.gotoAndPause(3000);
            },
            onHidden() {
                this.playerState = 'play';
                this.playerSeek = 0;
                this.$emit('hidden');
            },
            formatDuration(ms) {
                return AppHelper.msToHMS(ms);
            }
        }
    }
</script>

<style lang="scss">
    $lightGrey: #f5f5f5;
    #customize-video-modal {
        .modal-body {
            padding: 0px !important;
            .video-player {
                /*height: 560px;*/
            }
            .video-content {
                min-height: 100%;
                position: relative;
                padding: 15px;
                border-bottom: 1px solid $lightGrey;
                .title-wrap{
                    padding: 0px 0px 5px;
                    margin-bottom: 5px;
                    border-bottom: 1px solid $lightGrey;
                    h2{

                    }
                    div{
                        font-size: 1.3rem;
                    }
                }
                .description-wrap{
                    h2{
                        margin-bottom: 10px;
                    }
                }
                .start-customize-wrap{
                    position: absolute;
                    left: 15px;
                    right: 15px;
                    bottom: 15px;
                    .btn{
                        padding: 12px;
                        background-color: red;
                        color: white;
                        font-size: 20px;
                        font-weight: bold;
                    }
                }
            }
            .video-aside-wrap {
                .aside-item{
                    border-right: 1px solid $lightGrey;
                    padding: 10px 0px;
                    text-align: center;
                    font-size: 20px;
                    .fa{
                        font-size: 20px;
                        color: #5c565a;
                    }
                    .item-name{
                        font-size: 16px;
                        font-weight: bold;
                        color: grey;
                    }
                    .item-value{
                        font-size: 20px;
                    }
                    .full-hd{
                        padding: 2px 5px;
                        color: white;
                        background-color: #605e61;
                        font-style: italic;
                    }
                }
            }
            .video-variations {
                padding: 25px 25px 10px 25px;
                background-color: $lightGrey;
                .section-name{
                    font-size: 2rem;
                    margin-bottom: 15px;
                }
                .list-item{
                    display: flex;
                    .variation-item{
                        display: flex;
                        flex-basis: 420px;
                        margin-right: 20px;
                        .item-thumb{
                            flex-basis: auto;
                            min-width: 0px;
                            img{
                                width: 75px;
                                height: 100%;
                                /*object-fit: cover;*/
                                object-fit: initial;
                                object-position: center;
                            }
                        }
                        .item-content{
                            flex-grow: 1;
                            padding-left: 5px;
                            background-color: white;
                            border: 1px solid $lightGrey;
                            border-left: none;
                            font-size: 1.2rem;
                            .item-name{
                                padding: 10px;
                                font-size: 18px;
                                border-bottom: 1px solid $lightGrey;
                            }
                            .item-values{
                                padding: 5px 10px;
                                background-color: white;
                                i.fa{
                                    font-size: 20px;
                                    color: #585657;
                                }
                                .item-value{
                                    font-size: 15px;
                                }
                                span {
                                    font-size: 15px;
                                }

                            }
                        }
                    }
                }
            }
            .color-palette {
                padding: 10px 20px 40px 25px;
                background-color: $lightGrey;
                .section-name{
                    font-size: 2rem;
                    margin-bottom: 15px;
                }
                .list-item{
                    display: flex;
                    .style-item{
                        margin-right: 15px;
                        .input-wrap {
                            position: relative;
                            line-height: 0;
                            vertical-align: middle;
                            input{
                                position: absolute;
                                top: 0px;
                                left: 0px;
                                width: 100%;
                                height: 100%;
                                cursor: pointer;
                                opacity: 0;
                                & + label{
                                    width: 30px;
                                    height: 30px;
                                    margin-bottom: 0px;
                                    border-radius: 50%;
                                    border: 1px solid grey;
                                    background-color: white;
                                }
                                &:checked + label{
                                    background-color: blue;
                                }
                            }
                        }
                    }
                }
            }
        }
    }
</style>
