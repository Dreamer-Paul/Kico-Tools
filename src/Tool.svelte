<!-- 内置工具库 -->
<script context="module">
    // 补零
    export function zeroFill(num) {
        return num >= 10 ? num : "0" + num;
    }

    // 计次转时间
    export function countToTime(count) {
        let min, sec, ms, hour;

        ms = count % 100;
        sec = (count % 6000 - ms) / 100;
        min = (count % 360000 - sec * 100 - ms) / 6000;
        hour = (count % 8640000 - min * 6000 - sec * 100 - ms) / 360000;

        return zeroFill(min) + ":" + zeroFill(sec) + "." + zeroFill(ms);
    }

    // 时间转计次
    export function timeToCount(min, sec){
        return (min * 60 * 100) + (sec * 100);
    }

    // 获取当前时间
    export function getTime(){
        let date = new Date(), hour = date.getHours(), min = date.getMinutes(), sec = date.getSeconds();

        return [zeroFill(hour) + ":" + zeroFill(min) + ":" + zeroFill(sec), hour, min, sec];
    }

    // 保存记录的时间
    export function saveTime(arr){
        localStorage.setItem("kico-time", JSON.stringify(arr));
    }

    // 读取记录的时间
    export function loadTime(){
        const time = localStorage.getItem("kico-time");

        return time ? JSON.parse(time) : [];
    }
</script>
