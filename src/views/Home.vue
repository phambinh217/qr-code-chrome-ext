<template>
    <div class="qr-code">
        <div class="guide">
            Scan QRcode below to access this web page.
        </div>
        <qr-code :value="url" size="300" class="qr-code__content"></qr-code>
        <div class="input-group" style="margin-top: 30px;">
            <input v-model="url" class="qr-code__url input" id="url" />
            <button class="btn" @click="copy">Copy</button>
        </div>
        <div class="message" style="min-height: 13px;">{{ message }}</div>
    </div>
</template>
<script>
    import QrcodeVue from 'qrcode.vue';
    export default {
        components: {
            QrCode: QrcodeVue,
        },

        data () {
            return {
                url: 'https://phambinh.net',
                message: ''
            }
        },

        created () {
            if (process.env.NODE_ENV == 'production') {
                chrome.tabs.query({ 'active': true, 'lastFocusedWindow': true }, tabs => {
                    this.url = tabs[0].url
                })
            }
        },

        methods: {
            copy () {
                let copyText = document.getElementById('url')
                copyText.select()
                copyText.setSelectionRange(0, 99999)
                document.execCommand('copy')
                this.message = 'copied'
                let vm = this
                setTimeout(function () {
                    vm.message = ''
                }, 2000)
            }
        },

        watch: {
            url () {
                console.log(url)
            }
        }
    }
</script>
