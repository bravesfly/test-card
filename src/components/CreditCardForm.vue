<template>
    <div>
        <div>
            <div class="cardBg" data-tilt data-tilt-scale="1" data-tilt-max="7" data-tilt-full-page-listening
                data-tilt-reset="false" data-tilt-reverse="true"></div>
            <div class="card" id="card" data-tilt data-tilt-scale="1" data-tilt-max="7" data-tilt-full-page-listening
                data-tilt-reset="false" data-tilt-reverse="true" data-tilt-glare data-tilt-max-glare="0.1">
                <div class="bank">
                    <span id="cardBank">包成功</span>
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
                    <h1 id="cardValid">{{ expiryDate }}</h1>
                </div>

                <!-- <div class="c" style="color: #CCCCCC;">
                    <p>
                        <span>CVV</span>
                    </p>
                    <h1 id="cardValid">{{ expiryDate }}</h1>
                </div> -->

                <div class="name">
                    <p>
                        <span style="color: #cccccc;">Card holdername</span>
                    </p>
                    <span id="cardName">Mya Connell</span>
                </div>

                <div class="flag">
                    <img src="@/assets/images/wsd.png" id="flag" alt="Card Flag">
                </div>
            </div>

        </div>
        <div style="color: black;">
            <el-input v-model="text"></el-input>
        </div>
    </div>

</template>

<script>

export default {
    data() {
        return {
            cardNumber: '',
            expiryDate: '06/26',
            cvv: '',
            imageData: null,
            text: ''
        };
    },
    methods: {
    },
    computed: {
        cardSegments() {
            const segments = [];
            for (let i = 0; i < this.text.length; i += 4) {
                segments.push(this.text.substr(i, 4));
            }
            // Ensure the array has 4 elements
            while (segments.length < 4) {
                segments.push('');
            }
            return segments;
        }
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
    width: 70%;
}

.card>.name>span {
    font-size: 20px;
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