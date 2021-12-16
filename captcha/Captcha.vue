<template>
    <div id='captcha'>
        <input v-model="input_0" data-index='0' type="number" @input="inputCaptcha(0)" @keyup="back($event, 0)">
        <span class='sep'>-</span>
        <input v-model="input_1" data-index='1' type="number" @input="inputCaptcha(1)" @keyup="back($event, 1)">
        <span class='sep'>-</span>
        <input v-model="input_2" data-index='2' type="number" @input="inputCaptcha(2)" @keyup="back($event, 2)">
        <span class='sep'>-</span>
        <input v-model="input_3" data-index='3' type="number" @input="inputCaptcha(3)" @keyup="back($event, 3)"> 
        <span class='sep'>-</span>
        <input v-model="input_4" data-index='4' type="number" @input="inputCaptcha(4)" @keyup="back($event, 4)">
        <span class='sep'>-</span>
        <input v-model="input_5" data-index='5' type="number" @input="inputCaptcha(5)" @keyup="back($event, 5)">
        <input :value='result' id='captcha-result' hidden type="text">
        <button :disabled='disabled'>click</button>
    </div>
</template>
<script setup lang='ts'>
import { ref } from 'vue';
import { onMounted} from 'vue';
    // input组件的 v-model
    const [input_0, input_1, input_2, input_3,input_4, input_5] = [ref('') , ref(''), ref(''), ref(''), ref(''), ref('')]

    let inputs = {
        
    }
    // 最终结果
    const result = ref('');
    // 提交按钮是否可点击
    const disabled = ref(true)

    class Captcha {
        pre
        next
        input
        constructor(input, pre, next) {
            this.input = input;
            this.pre = document.querySelector(pre);
            this.next = document.querySelector(next);
        }
    }
    

    onMounted(() => {
        // 获取input dom
        inputs = {
            '0':  new Captcha(input_0, null, `#captcha input[data-index='1']`),
            '1': new Captcha(input_1, `#captcha input[data-index='0']`, `#captcha input[data-index='2']`),
            '2': new Captcha(input_2, `#captcha input[data-index='1']`, `#captcha input[data-index='3']`),
            '3': new Captcha(input_3, `#captcha input[data-index='2']`, `#captcha input[data-index='4']`),
            '4': new Captcha(input_4, `#captcha input[data-index='3']`, `#captcha input[data-index='5']`),
            '5': new Captcha(input_5, `#captcha input[data-index='4']`, null),
        }
    })
    // 获取最终验证码输入结果
    function getResult() {
        result.value =  `${input_0.value}${input_1.value}${input_2.value}${input_3.value}${input_4.value}${input_5.value}`;
        disabled.value = (result.value.length !== 6);
    }
    
    // 输入验证码时处理函数
    function inputCaptcha(index) {
        const captcha =  inputs[index]
        if (!captcha.input.value) {
            return;
        }
        captcha.input.value = Math.abs(captcha.input.value) > 10 ? Math.abs(Math.floor(captcha.input.value % 10)) : Math.abs(captcha.input.value);
        captcha.next?.focus();
        getResult()
    }

    // 删除验证码时处理函数
    function back(event , index) {
        if (event.key === 'Backspace') {
            const captcha = inputs[index];
            if (!captcha.input.value) {
                captcha.pre?.focus();
                getResult();
            }
        }
    }
    
</script>
<style lang="scss">
    #captcha {
        display: flex;
        align-items: center;
        .sep {
            margin: 5px;
        }
        /* 隐藏 input:number 组件的按钮 */
        input::-webkit-outer-spin-button,
        input::-webkit-inner-spin-button {
            appearance: none;
        }
        input[type="number"]{
            -moz-appearance: textfield;
        }

        input {
            width: 30px;
            height: 30px;
            text-align: center;
        }
        input:focus {
            border: 1px solid black;
        }
    }
</style>