<template>
    <div class="processbar-container">
        <div class="info">
            <h2 class="title">{{`Updating ${total} files`}}</h2>
            <div class="details">
                <span class="percent">{{processPercent}}</span>
                <span class="status">
                    {{isPaused ? 'Paused' : `${remainTime | toInt} seconds left`}}
                </span>
            </div>
        </div>
        <div class="buttons">
            <div class="button circle pause" @click="handleClick">
                <img svg-inline class="icon" src="./img/pause.svg" />
                <img svg-inline class="icon" src="./img/loading.svg" />
            </div>
            <div class="button circle close">
                <img svg-inline class="icon" src="./img/close.svg" alt="closeIcon" />
            </div>
            <div class="button expand">
                <img svg-inline class="icon" src="./img/expand.svg" alt="expandIcon" />
            </div>
            <div class="button more">
                <img svg-inline class="icon" src="./img/more.svg" alt="moreIcon" />
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
    filter: {
        toInt(value) {
            return value.toFixed(0);
        }
    },
    computed: {
        processPercent() {
            return (100 - this.remainTime / this.totalTime * 100).toFixed(0) + '%'
        }
    },
    methods: {
        handleClick() {
            this.isPaused = ! this.isPaused;
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
    width: 80%;
    height: 10rem;
    background-color: white;
    border-radius: 15px;
    -webkit-box-shadow: 3px 3px 10px 10px #eeeeee;
    box-shadow: 3px 3px 10px 10px #eeeeee;
    margin: 5% auto;
    padding: 5px;
    display: flex;
    justify-content: space-around;
    align-items: center;
    position: relative;
    overflow: hidden;
    .info {
        width: 50%;
        height:80%;
        .title {

        }
        .details {

        }
    }
    .buttons {
        width: 40%;
        height: 80%;
        display: flex;
        justify-content: space-around;
        align-items: center;
        .button {
            width: 5rem;
            height: 5rem;
            line-height: 5rem;
            text-align: center;
            display: flex;
            justify-content: center;
            align-items: center;
            svg {
                width: 3.5rem;
                height: 3.5rem;
            } 
            &.circle {
                border-radius: 50%;
                background-color: rgba(155,175,196,0.5);
                color: white;
                width: 3.5rem;
                height: 3.5rem;
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
    }
}
</style>
