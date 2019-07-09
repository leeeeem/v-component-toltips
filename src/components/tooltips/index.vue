<template>
    <div class="tooltips" :style="boxStyle">

        <div :class="hoverClass" :style="tooltipStyle">
            <div class="content-inline" v-if="content">{{content}}</div>
            <slot v-else name="content"></slot>
        </div>

        <div class="tooltips-original" @mouseover="onMouseover" @mouseout="onMouseout">
            <slot></slot>
        </div>

    </div>
</template>

<script>
    export default {
        name: 'Tooltips',
        data() {
            return {
                changeColorTrigger: false,
                visible: false,
                timeoutId: null
            }
        },
        props: {
            placement: {
                type: String,
                default: 'bottom'
            },
            noArrow: {
                type: Boolean,
                default: false
            },
            duration: {
                type: Number,
                default: 0
            },
            transitionFunction: {
                type: String,
                default: "ease-in"
            },
            //offset为2元数组，为方向为[上，左]的偏移量
            offset: {
                type: Array,
                default: () => {
                    return [0, 0]
                }
            },
            showDelay: {
                type: Number,
                default: 0
            },
            hideDelay: {
                type: Number,
                default: 0
            },
            content: {
                type: String,
                default: null
            },
            disabled: {
                type: Boolean,
                default: false
            },
            textColor: {
                type: String,
                default: "#66009D"
            },
            boxColor: {
                type: String,
                default: "#7700BB"
            }
        },
        methods: {
            onMouseover: function () {
                this.changeColorTrigger = true
                if (this.timeoutId) {
                    clearTimeout(this.timeoutId)
                }
                this.timeoutId = setTimeout(() => {
                    this.visible = true
                    this.$emit("onPopperOpen")
                }, this.showDelay * 1000)
            },
            onMouseout() {
                this.changeColorTrigger = false
                this.timeoutId = setTimeout(() => {
                    this.visible = false
                    this.$emit("onPopperClose")
                }, (this.hideDelay + this.showDelay) * 1000)
            }
        },
        computed: {
            boxStyle() {
                if (this.changeColorTrigger === true) {
                    return `border: 1px solid ${this.boxColor}; color: ${this.textColor};`
                } else {
                    return ''
                }
            },
            hoverClass() {
                let hoverIndex = "top, bottom, right, left, " +
                    "top-right, top-left, bottom-left, bottom-right, " +
                    "left-top, left-bottom, right-top, right-bottom"
                let result = {
                    "common": true,
                    "no-arrow": this.noArrow,
                    "top": false,
                    "bottom": false,
                    "right": false,
                    "left": false,
                    "top-left": false,
                    "top-right": false,
                    "bottom-left": false,
                    "bottom-right": false,
                    "left-top": false,
                    "left-bottom": false,
                    "right-top": false,
                    "right-bottom": false,
                    "open-delay": false,
                    "hide-after": false,
                    "disabled": this.disabled && !this.visible,
                    "visible": this.visible && !this.disabled
                }
                if (hoverIndex.indexOf(this.placement) === -1) {
                    result["bottom"] = true
                } else {
                    result[this.placement] = true
                }
                return result
            },
            tooltipStyle() {
                let result = {
                    transition: `all ${this.duration}s ${this.transitionFunction}`
                }
                let realOffset = 10
                if (!this.visible) {
                    result["margin"] = `0 0 0 0`
                } else if (this.placement === "top" || this.placement === "top-left" || this.placement === "top-right") {
                    result["margin"] = `0 0 ${this.offset[0] ? realOffset - this.offset[0] : realOffset}px ${this.offset[1] ? this.offset[1] : 0}px`
                } else if (this.placement === "bottom" || this.placement === "bottom-left" || this.placement === "bottom-right") {
                    result["margin"] = `${this.offset[0] ? realOffset + this.offset[0] : realOffset}px 0 0 ${this.offset[1] ? this.offset[1] : 0}px`
                } else if (this.placement === "left" || this.placement === "left-top" || this.placement === "left-bottom") {
                    result["margin"] = `${this.offset[0] ? this.offset[0] : 0}px ${this.offset[1] ? realOffset - this.offset[1] : realOffset}px 0 0`
                } else if (this.placement === "right" || this.placement === "right-top" || this.placement === "right-bottom") {
                    result["margin"] = `${this.offset[0] ? this.offset[0] : 0}px 0 0 ${this.offset[1] ? realOffset + this.offset[1] : realOffset + 0}px`
                }
                return result
            }
        }
    }
</script>

<style lang="scss" scoped>
    .tooltips {
        position: relative;
        display: inline-block;
        border-radius: 5px;
        border: 1px solid rgba(196, 196, 196, 1);

        .tooltips-original {
            display: flex;
            cursor: pointer;
        }

        .common {
            visibility: hidden;
            position: absolute;
            opacity: 0;
            z-index: 1;
            padding: 10px;
            text-align: center;
            border-radius: 6px;
            color: rgba(255, 255, 255, 1);
            background-color: rgba(31, 45, 61, 1);

            &.visible {
                visibility: visible;
                cursor: default;
                opacity: 1;
            }

            .content-inline {
                white-space: nowrap;
            }

            &:after {
                position: absolute;
                content: " ";
                border-width: 5px;
                border-style: solid;
            }

            &.no-arrow:after {
                border-width: 0;
            }
        }

        .top {
            bottom: 100%;
            left: 50%;
            transform: translate(-50%, 0);

            &:after {
                top: 100%;
                left: 50%;
                transform: translate(-50%, 0);
                border-color: rgba(31, 45, 61, 1) transparent transparent transparent;
            }
        }


        .bottom {
            top: 100%;
            left: 50%;
            transform: translate(-50%, 0);

            &:after {
                bottom: 100%;
                left: 50%;
                transform: translate(-50%, 0);
                border-color: transparent transparent rgba(31, 45, 61, 1) transparent;
            }
        }

        .right {
            top: 50%;
            left: 100%;
            transform: translate(0, -50%);

            &:after {
                top: 50%;
                right: 100%;
                transform: translate(0, -50%);
                border-color: transparent rgba(31, 45, 61, 1) transparent transparent;
            }
        }

        .left {
            top: 50%;
            right: 100%;
            transform: translate(0, -50%);

            &:after {
                top: 50%;
                left: 100%;
                transform: translate(0, -50%);
                border-color: transparent transparent transparent rgba(31, 45, 61, 1);
            }
        }

        .top-left {
            bottom: 100%;
            left: 0;

            &:after {
                top: 100%;
                left: 0;
                transform: translate(50%, 0);
                border-color: rgba(31, 45, 61, 1) transparent transparent transparent;
            }
        }

        .top-right {
            bottom: 100%;
            right: 0;

            &:after {
                top: 100%;
                right: 0;
                transform: translate(-50%, 0);
                border-color: rgba(31, 45, 61, 1) transparent transparent transparent;
            }
        }

        .bottom-left {
            top: 100%;
            left: 0;

            &:after {
                bottom: 100%;
                left: 0;
                transform: translate(50%, 0);
                border-color: transparent transparent rgba(31, 45, 61, 1) transparent;
            }
        }

        .bottom-right {
            top: 100%;
            right: 0;

            &:after {
                bottom: 100%;
                right: 0;
                transform: translate(-50%, 0);
                border-color: transparent transparent rgba(31, 45, 61, 1) transparent;
            }
        }

        .left-top {
            top: 0;
            right: 100%;
            transform: translate(0, 0);

            &:after {
                top: 0;
                left: 100%;
                transform: translate(0, 50%);
                border-color: transparent transparent transparent rgba(31, 45, 61, 1);
            }
        }

        .left-bottom {
            bottom: 0;
            right: 100%;
            transform: translate(0, 0);

            &:after {
                bottom: 0;
                left: 100%;
                transform: translate(0, -50%);
                border-color: transparent transparent transparent rgba(31, 45, 61, 1);
            }
        }

        .right-top {
            top: 0;
            left: 100%;
            transform: translate(0, 0);

            &:after {
                top: 0;
                right: 100%;
                transform: translate(0, 50%);
                border-color: transparent rgba(31, 45, 61, 1) transparent transparent;
            }
        }


        .right-bottom {
            top: 100%;
            left: 100%;
            transform: translate(0, -100%);

            &:after {
                bottom: 0;
                right: 100%;
                transform: translate(0, -50%);
                border-color: transparent rgba(31, 45, 61, 1) transparent transparent;
            }
        }
    }
</style>
