<template>
    <div class="processbar-container">
        <div class="info">
            <div class="title">{{`Updating ${total} files`}}</div>
            <div class="details">
                <span class="percent">{{processPercent}}</span>
                <span class="dot"> &middot; </span>
                <span class="status">
                    {{isPaused ? 'Paused' : remainTimeString}}
                </span>
            </div>
        </div>
        <div class="buttons">
            <div class="button circle toggle" :class="isPaused ? 'pause' : ''" @click="handleClick" @mousedown="handleMouseDown" @mouseup="handleMouseUp">
                <div class="svg-container">
                    <div class="pause-svg-container">
                         <img svg-inline class="icon" src="./img/pause.svg" />
                     </div>
                     <div class="loading-svg-container">
                         <img svg-inline class="icon" src="./img/loading.svg" />
                     </div>
                </div>
            </div>
            <div class="button circle close">
                <div class="svg-container">
                    <img svg-inline class="icon" src="./img/close.svg" alt="closeIcon" />
                </div>
            </div>
            <div class="button expand">
                <div class="svg-container">
                    <img svg-inline class="icon" src="./img/expand.svg" alt="expandIcon" />
                </div>
            </div>
            <div class="button more">
                <div class="svg-container">
                    <img svg-inline class="icon" src="./img/more.svg" alt="moreIcon" />
                </div>
            </div>
        </div>
        <div class="animation" :style="{width: processPercent}"></div>
    </div>
</template>
<script>
import { setInterval, clearInterval } from 'timers';

export default {
    props: {
        total: {
            type: Number,
            default: 3
        },
        totalTime: {
            type: Number,
            default: 10
        }
    },
    data() {
        return {
            remainTime: this.totalTime,
            isPaused: false,
            mockProcess: null
        }
    },
    computed: {
        processPercent() {
            return (100 - this.remainTime / this.totalTime * 100).toFixed(0) + '%'
        },
        remainTimeString() {
            return this.remainTime.toFixed(0) + ' seconds left';
        }
    },
    methods: {
        handleClick() {
            this.isPaused = ! this.isPaused;
        },
        handleMouseDown() {
            let buttonDom = document.querySelectorAll('.button')[0];
            buttonDom.style.transform = 'scale(0.75)';
        },
        handleMouseUp() {
            let buttonDom = document.querySelectorAll('.button')[0];
            buttonDom.style.transform = 'scale(1)';
        }
    },
    mounted() {
        this.mockProcess = setInterval(() => {
            if ( this.processPercent === '100%') {
                clearInterval(this.mockProcess);
                return;
            }
            if ( !this.isPaused ) {
                this.remainTime -= 0.1;                
            }
        }, 100);
    },
    beforeDestroy() {
        if (this.mockProcess) {
            clearInterval(this.mockProcess);
        }
    }
}
</script>
<style lang="scss" scoped>
.processbar-container {
    width: 36rem;
    height: 8rem;
    background-color: white;
    border-radius: 10px;
    -webkit-box-shadow: 3px 3px 15px 15px #eaeaea;
    box-shadow: 3px 3px 15px 15px #eaeaea;
    margin: 5% auto;
    padding: 5px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: relative;
    overflow: hidden;
    .info {
        width: 40%;
        height:80%;
        display: flex;
        justify-content: flex-start;
        align-items: flex-start;
        flex-direction: column;
        z-index: 1;
        padding-left: 1.8rem;
        .title {
            width: 90%;
            font-size: 1.5rem;
            height: 3rem;
            line-height: 3rem;
            text-align: left;
        }
        .details {
            width: 90%;
            height: 2rem;
            line-height: 2rem;
            text-align: left;
            font-size: 1.2rem;
            color: #909090;
        }
    }
    .buttons {
        width: 40%;
        height: 80%;
        display: flex;
        justify-content: space-around;
        align-items: center;
        z-index: 1;
        .button {
            width: 5rem;
            height: 5rem;
            line-height: 5rem;
            text-align: center;
            display: flex;
            justify-content: center;
            align-items: center;
            transition: all 0.2s;
            transition-timing-function: ease-out;
            position: relative;
            cursor: pointer;
            .svg-container {
                width: 3rem;
                height: 3rem;
                display: flex;
                justify-content: center;
                align-items: center;
                position: relative;
            }
            .pause-svg-container, .loading-svg-container {
                position:absolute;
                top: 0;
                left: 0;
                width: 100%;
                height: 100%;
                display: flex;
                justify-content: center;
                align-items: center;
            }
            .pause-svg-container {
                opacity: 1;
                transition: all 1s;
            }
            .loading-svg-container {
                opacity: 0;
                transition: all 0.4s;
                transform: rotate(180deg);
            }
            &.pause {
                .pause-svg-container {
                    transition: all 0.4s;
                    opacity: 0;
                }
                .loading-svg-container {
                    transition: all 1s;
                    opacity: 1;
                    transform: rotate(0deg);
                }
            }
            svg {
                width: 2.2rem;
                height: 2.2rem;
                path {
                    fill: #908a9e;
                }
            } 
            &.circle {
                opacity: 0;
                transform: translate(0rem,1rem);
                .svg-container {
                    margin: 0 0.6rem;
                    border-radius: 50%;
                    background-color: rgba(155,175,196,0.2);
                    color: white;
                    &:hover {
                        background-color: rgba(155,175,196,0.3);
                    }
                }
            }           
        }
    }
    .animation {
        position: absolute;
        height: 100%;
        top: 0;
        left: 0;
        background-color: rgba(155,175,196,0.1);
        transition: all 0.1s;
        z-index: 0;
    }
    &:hover {
        .button.circle {
            opacity: 1;
            transform: translate(0,0);
        }
    }
}
</style>
