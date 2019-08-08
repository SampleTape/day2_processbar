<template>
    <div class="processbar-container">
        <div class="title">{{`Updating ${total} files`}}</div>
        <div class="details">
            <span class="percent">{{processPercent}}</span>
            <span class="status">
                {{isPaused ? 'Paused' : `${remainTime} seconds left`}}
            </span>
        </div>
        <div class="buttons">
            <div class="pause" @click="handleClick">
                {{isPaused ? 'start' : 'pause'}}
            </div>
            <div class="cancel">
                cancel
            </div>
            <div class="expand">
                expand
            </div>
            <div class="more">
                more
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
                this.remainTime --;                
            }
        }, 1000);
    },
    beforeDestroy() {
        if (this.mockProcess) {
            clearInterval(this.mockProcess);
        }
    }
}
</script>
<style lang="scss" scoped>

</style>
