<script setup>
import { reactive, ref } from 'vue';
const props = defineProps({
    taskId: {
        type: Number,
        required: true
    }
})

const task1 = reactive({
    limit1: "",
    limit2:""
});
const task2 = reactive({
    string:""
});
const task3 = reactive({
    number:""
});
const result = ref("");

async function onSubmitClick() {
    if(props.taskId==1){
        if (!/^\d+$/.test(task1.limit1) || !/^\d+$/.test(task1.limit2)) {
            alert('Please enter valid integer in both limit1 and limit2');
            return;
        }
        const min = task1.limit1 < task1.limit2 ? task1.limit1 : task1.limit2;
        const max = task1.limit1 > task1.limit2 ? task1.limit1 : task1.limit2;
        result.value = await task1Solution(min,max)
    }
    else if(props.taskId==2){
        if(!task2.string){
            alert("Please enter the string.");
            return;
        }
        result.value = await task2Solution(task2.string);
    }
    else if(props.taskId==3){
        if (!/^\d{3}$/.test(task3.number)) {
            alert('Please enter a 3-digit number.');
            return;
        }
        result.value = task3Solution(task3.number);
    }
}

function task1Solution(limit1,limit2) {

    const results = [];
    for (let i = limit1; i <= limit2; i++) {
        if (i === parseInt(i.toString().split('').reverse().join(''))) {
            results.push(i);
        }
    }
    return results.join(',');
}

function task2Solution(string) {
    let resultString = "";
    const stringLength = string.length;
    for(let i=stringLength-1;i>=0;i--){
        resultString += string[i];
    }
    return resultString
}

function task3Solution(number) {
    let digits = [number%10, Math.floor(number/10)%10, Math.floor(number/100)%10];
    const digitCount = [0,0,0,0,0,0,0,0,0,0];

    for (const digit of digits) {
        digitCount[digit]++;
    }

    let result = 0;

    for (let num = 100; num < 999; num++) {
        const numCount = [0,0,0,0,0,0,0,0,0,0];
        let temp = num;

        while (temp) {
            numCount[temp % 10]++;
            temp = Math.floor(temp / 10);
        }

        let isPossible = true;

        for (let i = 0; i < 10; i++) {
            if (numCount[i] > digitCount[i]) {
                isPossible = false;
                break;
            }
        }

        if (isPossible) {
        result += 1;
        }
    }

    return result;
}
</script>

<template>
    <div>
        <div v-if="taskId==1" class="m-2">
            <div class="row">
                <hr class="text-primary">
                <h6 class="col text-center">
                    List of number that remains the same when its digits are reversed in a given limit.
                </h6>
                <hr class="text-primary">
            </div>
            <div class="row">
                <div class="col-12 py-3">
                    <span class="text-danger">Fields marked with an asterisk (*) are required.</span>
                </div>
            </div>
            <div class="row">
                <div class="col-12">
                    <div class="input-group mb-3">
                        <span class="input-group-text" id="limit1">Limit 1 <span class="text-danger">*</span></span>
                        <input type="number" class="form-control" placeholder="Lower Limit" aria-label="Lower Limit" aria-describedby="limit1" v-model="task1.limit1">
                    </div>
                </div>
                <div class="col-12">
                    <div class="input-group mb-3">
                        <span class="input-group-text" id="limit2">Limit 2<span class="text-danger">*</span></span>
                        <input type="number" class="form-control" placeholder="Upper Limit" aria-label="Upper Limit" aria-describedby="limit2" v-model="task1.limit2">
                    </div>
                </div>
            </div>
        </div>
        <div v-else-if="taskId==2" class="m-2">
            <div class="row">
                <hr class="text-primary">
                <h6 class="col text-center">
                    Reverse words and characters in a given string.
                </h6>
                <hr class="text-primary">
            </div>
            <div class="row">
                <div class="col-12 py-3">
                    <span class="text-danger">Fields marked with an asterisk (*) are required.</span>
                </div>
            </div>
            <div class="row">
                <div class="col-12">
                    <div class="input-group mb-3">
                        <span class="input-group-text" id="stringInput">String <span class="text-danger">*</span></span>
                        <input type="text" class="form-control" aria-label="String Input" aria-describedby="stringInput" required placeholder="Enter the String" v-model="task2.string">
                    </div>
                </div>
            </div>
        </div>
        <div v-else-if="taskId==3" class="m-2">
            <div class="row">
                <hr class="text-primary">
                <h6 class="col text-center">
                    Find number of possible unique combinations for a 3 digit number
                </h6>
                <hr class="text-primary">
            </div>
            <div class="row">
                <div class="col-12 py-3">
                    <span class="text-danger">Fields marked with an asterisk (*) are required.</span>
                </div>
            </div>
            <div class="row">
                <div class="col-12">
                    <div class="input-group mb-3">
                        <span class="input-group-text" id="threeDigit">3-digit Number <span class="text-danger">*</span></span>
                        <input type="number" maxlength="3" class="form-control" aria-label="3 digit number" aria-describedby="threeDigit" required placeholder="Enter a 3-digit number" v-model="task3.number">
                    </div>
                </div>
            </div>
        </div>
        <div v-else>
            <h5 class="text-center m-4"><strong>Task {{ taskId }}</strong> <small>is not active.</small></h5>
        </div>
        <div v-if="taskId == 1 || taskId ==2 || taskId == 3" id="footer" class="mt-3">
            <div class="row">
                <div class="col-12 text-center">
                    <button class="btn btn-primary btn-sm rounded" @click="onSubmitClick()">Submit</button>
                </div>
            </div>
            <div class="row">
                <div class="col-12">
                    <hr class="text-primary">
                </div>
            </div>
            <div class="row">
                <div class="col-12">
                    <h5 class="ms-2"><u>Result</u></h5>
                </div>
                <div class="col-12">
                    <div class="mx-2 my-3 p-2 border border-primary rounded">
                        {{ result }}
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
</style>