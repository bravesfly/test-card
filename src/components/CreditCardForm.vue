<template>
    <div>
        <div>
            <!-- <div class="cardBg" data-tilt data-tilt-scale="1" data-tilt-max="7" data-tilt-full-page-listening
                data-tilt-reset="false" data-tilt-reverse="true"></div> -->
                <div class="card" id="card" data-tilt data-tilt-scale="1" data-tilt-max="7"
                    data-tilt-full-page-listening data-tilt-reset="false" data-tilt-reverse="true" data-tilt-glare
                    data-tilt-max-glare="0.1">
                    <div class="bank">
                        <span id="cardBank">{{ form.issue }}</span>
                    </div>

                    <div class="chip">
                        <div class="chip-risk" id="chip-1"></div>
                        <div class="chip-risk" id="chip-2"></div>
                        <div class="chip-risk" id="chip-3"></div>
                        <div class="chip-risk" id="chip-4"></div>
                    </div>

                    <div class="number">
                        <span id="cardNumber-1">{{ cardSegments[0] }}</span>
                        <span id="cardNumber-2">{{ cardSegments[1] }}</span>
                        <span id="cardNumber-3">{{ cardSegments[2] }}</span>
                        <span id="cardNumber-4">{{ cardSegments[3] }}</span>
                    </div>

                    <div class="valid">
                        <p>
                            <span>GOOD</span>
                            <span>THRU</span>
                        </p>
                        <h1 id="cardValid">{{ `${form.expireMonth}/${formatNumber(form.expireYear)}` }}</h1>
                    </div>

                    <div class="valid" style="left: 200px;">
                        <p>
                            <span style="font-size: medium;">CVV</span>
                        </p>
                        <h1 id="cardValid">{{ form.cvv }}</h1>
                    </div>
                    <div class="name">
                        <span id="cardName">{{ form.cardHolder }}</span>
                    </div>

                    <div class="flag">
                        <img src="@/assets/images/wsd.png" id="flag" alt="Card Flag">
                    </div>
                </div>


        </div>
        <div style="position: absolute;left: 50%;transform: translateX(-50%);top:360px;width: 500px;">
            <el-button @click="takeScreenshot" type="success" style="width: 100%;margin-bottom: 15px;"> Save</el-button>
            <el-form ref="form" :model="form" label-width="80px">
                <el-form-item label="Number">
                    <el-input v-model="form.number"></el-input>
                </el-form-item>
                <el-form-item label="CVV">
                    <el-input v-model="form.cvv"></el-input>
                </el-form-item>
                <el-form-item label="Expire">
                    <el-row :gutter="5">
                        <el-col :span="11"><el-select v-model="form.expireMonth">
                                <el-option v-for="month in 12" :key="month" :label="month < 10 ? '0' + month : month"
                                    :value="month"></el-option>
                            </el-select></el-col>
                        <el-col :span="12"><el-select v-model="form.expireYear">
                                <el-option v-for="year in years" :key="year" :label="year" :value="year"></el-option>
                            </el-select></el-col>
                    </el-row>
                </el-form-item>
                <el-form-item label="Holder">
                    <el-input v-model="form.cardHolder"></el-input>
                </el-form-item>
                <el-form-item label="Issue">
                    <el-input v-model="form.issue"></el-input>
                </el-form-item>
            </el-form>
        </div>
    </div>

</template>

<script>
import html2Canvas from 'html2canvas';
export default {
    data() {
        return {
            cardNumber: '',
            expiryDate: '06/26',
            cvv: '',
            imageData: null,

            form: {
                issue: '包成功',
                number: '',
                cvv: '',
                expireMonth: '06',
                expireYear: '2026',
                cardHolder: 'Ameno Code'
            },
            currentYear: new Date().getFullYear()
        };
    },
    methods: {
        formatNumber(number) {
            return number.toString().slice(-2);
        },
        takeScreenshot() {
            let htmlDom = document.getElementById("card");
            // let that = this;
            html2Canvas(htmlDom, {
                allowTaint: true,
                useCORS: true,
                scrollY: 0,
                scrollX: 0,
            }).then(function (canvas) {
                let dataURL = canvas.toDataURL("image/png");
                console.log(dataURL);
                                    var a = document.createElement('a')
                    a.download = "分析报告";
                    a.href = dataURL;
                    a.click()
            });
        },
    },
    watch: {
        'form.number': function (newVal) {
            // 定义正则表达式
            let cvv = /^\d{16}\/\d{3}$/;
            let expire = /^\d{16}\/\d{2}\/\d{2}\/\d{3}$/;
            let expireFull = /^\d{16}\/\d{2}\/\d{4}\/\d{3}$/;
            let result1 = newVal.match(cvv);
            let result2 = newVal.match(expire);
            let result3 = newVal.match(expireFull);

            // 检查匹配结果并打印捕获组内容
            if (result1) {
                this.form.number = newVal.split('/')[0]
                this.form.cvv = newVal.split('/')[1]
            }
            if (result2) {
                this.form.number = newVal.split('/')[0]
                this.form.expireMonth = newVal.split('/')[1]
                this.form.expireYear = '20' + newVal.split('/')[2]
                this.form.cvv = newVal.split('/')[3]
            }
            if (result3) {
                this.form.number = newVal.split('/')[0]
                this.form.expireMonth = newVal.split('/')[1]
                this.form.expireYear = newVal.split('/')[2]
                this.form.cvv = newVal.split('/')[3]
            }
        }
    },
    computed: {
        years() {
            // 生成从当前年份到十年之后的年份的数组
            const yearsArray = [];
            for (let year = this.currentYear; year <= this.currentYear + 10; year++) {
                yearsArray.push(year);
            }
            return yearsArray;
        },
        cardSegments() {
            const segments = [];
            for (let i = 0; i < this.form.number.length; i += 4) {
                segments.push(this.form.number.substr(i, 4));
            }
            // Ensure the array has 4 elements
            while (segments.length < 4) {
                segments.push('');
            }
            return segments;
        },
    },
};

</script>

<style>
/* CARD */
.cardBg {
    margin: 0 auto;

    width: 500px;
    height: 300px;

    border-radius: 10px;

    position: fixed;
    top: 25px;
    left: 50%;
    margin-left: -250px;

    z-index: 4;

    background: rgba(255, 255, 255, 0.2);
    border-radius: 16px;
    box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
    backdrop-filter: blur(5px);
    -webkit-backdrop-filter: blur(5px);
}

.card {
    margin: 0 auto;

    width: 500px;
    height: 300px;

    border-radius: 10px;

    position: fixed;
    top: 25px;
    left: 50%;
    margin-left: -250px;

    z-index: 5;

    user-select: none;

    background: var(--card-color-1);
    background: -moz-linear-gradient(150deg, var(--card-color-2) 0%, var(--card-color-2) 100%);
    background: -webkit-linear-gradient(150deg, var(--card-color-1) 0%, var(--card-color-2) 100%);
    background: linear-gradient(150deg, var(--card-color-1) 0%, var(--card-color-2) 100%);
    filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#cb2d3e", endColorstr="#ef4744", GradientType=1);
}

.card>.bank {
    position: absolute;
    top: 10px;
    right: 20px;
}

.card>.bank>span {
    font-family: 'Maven Pro', sans-serif;
    font-weight: 700;
    font-size: 45px;

    color: white;
}

.card>.chip {
    position: absolute;

    top: 45%;

    width: 50px;
    height: 40px;
    background: rgb(207, 178, 44);
    background: linear-gradient(150deg, rgba(207, 178, 44, 1) 0%, rgba(255, 224, 100, 1) 31%, rgba(235, 188, 65, 1) 100%);
    border-radius: 5px;

    transform: translate(33px, -40px);
    overflow: hidden;
}

.chip-risk {
    width: 50px;
    height: 40px;
    border-radius: 10px;
    border: 1px solid rgb(228, 228, 228);
    position: absolute;
    z-index: 0;
}

#chip-1 {
    transform: translate(37px, 15px);
}

#chip-2 {
    transform: translate(15px, 30px);
}

#chip-3 {
    transform: translate(-37px, -15px);
}

#chip-4 {
    transform: translate(-15px, -30px);
}

.card>.number {
    position: absolute;
    top: 50%;
    left: 30px;

    width: 70%;
}

.card>.number>span {
    font-size: 30px;
    font-weight: 500;

    background-color: rgb(204, 204, 204);
    color: transparent;
    text-shadow: -1px -1px 1px rgba(255, 255, 255, 0.2);
    -webkit-background-clip: text;
    -moz-background-clip: text;
    background-clip: text;
}

.card>.number>span+span {
    margin-left: 15px;
}

.card>.valid {
    position: absolute;
    top: 63%;
    left: 30px;

    display: grid;
    grid-template-columns: 1fr 1.5fr;
    gap: 10px;
    align-items: center;
}

.card>.valid>p {
    text-align: center;
    background-color: rgb(231, 231, 231, 0.7);
    color: transparent;
    text-shadow: -1px -1px 1px rgba(255, 255, 255, 0.2);
    -webkit-background-clip: text;
    -moz-background-clip: text;
    background-clip: text;
}

.card>.valid>p>span {
    text-align: right;
    display: block;
    font-size: 10px;
}

.card>.valid>p>span:last-child {
    font-size: 9px;
}

.card>.valid>h1 {
    background-color: rgba(255, 255, 255, 0.7);
    color: transparent;
    text-shadow: -1px -1px 1px rgba(255, 255, 255, 0.2);
    -webkit-background-clip: text;
    -moz-background-clip: text;
    background-clip: text;

    font-size: 20px;
}

.card>.name {
    position: absolute;
    top: 75%;
    left: 30px;
    width: 50%;
}

.card>.name>span {
    font-size: 24px;
    font-weight: 700;
    letter-spacing: 1px;
    word-spacing: 10px;

    background-color: rgb(204, 204, 204);
    color: transparent;
    text-shadow: -1px -1px 1px rgba(255, 255, 255, 0.2);
    -webkit-background-clip: text;
    -moz-background-clip: text;
    background-clip: text;
}

.card>.flag {
    position: absolute;
    bottom: 10px;
    right: 15px;
}

.card>.flag>img {
    width: 66.6666666667px;
    height: 50px;
}
</style>