<template>
    <div class="qr-wrapper">
        <h2> QR-код на портфолио</h2>
        <canvas ref="canvas"></canvas>
        <p>Отсканируйте QR код</p>
        <button class="visit-btn" @click="'visitSite'">Перейти по QR-коду</button>
    </div>
</template>

<script>
import QRCode from 'qrcode'

export default {
    name : 'QRCode',
    data(){
        return {
            url: 'http://github.com/SaintVl'
        }
    },
    mounted(){
        const canvas = this.$refs.canvas; 
        if (canvas){
            QRCode.toCanvas(canvas, this.url, {width: 180}, (error) => {
                if(error) console.error (error)
            })
        }
    },
    methods: {
        visitSite (){
            window.open(this.url, '_blank')
        }
    }
}

</script>