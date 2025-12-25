<template>
    <div class="page-footer" v-if="!disableFooter">
        <p style="color: cornflowerblue; font-weight: bold;">
            <a :href="footerLink" target="_blank">
                <font-awesome-icon icon="paper-plane" class="footer-link-icon"/>
            </a>    本站已运行：
        </p>
        <span style="color: deepskyblue; font-weight: bold;">{{ runningTime }}</span>
    </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
    name: 'Footer',
    computed: {
        ...mapGetters(['userConfig']),
        footerLink() {
            return this.userConfig?.footerLink || 'https://blog.o0w0b.top/posts/2ad8ecff/'
        },
        thisYear() {
            return new Date().getFullYear()
        },
        disableFooter() {
            return this.userConfig?.disableFooter || false
        }
    },
    data() {
        return {
        runningTime: '',   // 运行时间文本
        };
    },
    methods: {
        // 更新运行时间
        updateTime() {
            var start = new Date("2025/02/01 19:26:11");
            var now = new Date();

            var years = now.getFullYear() - start.getFullYear();
            var anniversary = new Date(start);
            anniversary.setFullYear(start.getFullYear() + years);
            if (now < anniversary) {
                years--;
                anniversary.setFullYear(start.getFullYear() + years);
            }

            var diff = now - anniversary;
            var dayMs = 24 * 60 * 60 * 1000;
            var hourMs = 60 * 60 * 1000;
            var minMs = 60 * 1000;

            var days = Math.floor(diff / dayMs);
            diff %= dayMs;
            var hours = Math.floor(diff / hourMs);
            diff %= hourMs;
            var mins = Math.floor(diff / minMs);
            diff %= minMs;
            var secs = Math.floor(diff / 1000);
            this.runningTime = `${years} 年 ${days} 天 ${hours} 小时 ${mins} 分 ${secs} 秒`;
        },
    },
    mounted() {
        // 每秒更新一次运行时间
        this.updateTime(); // 初始化调用一次
        setInterval(this.updateTime, 1000);
    },
}
</script>

<style scoped>
.page-footer {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100vw;
    color: var(--page-footer-text-color);
    font-size: large;
    user-select: none;
}
@media (max-width: 768px) {
    .page-footer {
        font-size: small;
    }
}
.footer-name {
    color: var(--page-footer-name-color);
    font-weight: bold;
    text-decoration: none;
    position: relative;
    transition: all 0.3s ease;
}

.footer-name::after {
    content: '';
    position: absolute;
    width: 0;
    height: 2px;
    bottom: -2px;
    left: 0;
    background-color: var(--page-footer-name-color);
    transition: width 0.3s ease-in-out;
}

.footer-name:hover::after {
    width: 100%;
}

.footer-link-icon {
    color: var(--page-footer-name-color);
    margin-left: 5px;
    transition: transform 0.3s ease-in-out;
}

.footer-link-icon:hover {
    transform: scale(1.2) rotate(-12deg);
}
</style>