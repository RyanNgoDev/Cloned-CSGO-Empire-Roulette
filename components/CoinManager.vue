<template>
    <div class="border flex justify-between px-2 py-1 border-slate-500">
        <span class="inline-block align-middle m-auto">{{ coins }}</span>
        <Transition name="input-fade">
            <input class="bg-black ml-2 w-20 appearance-none" v-model="inputCoin" v-if="isShowInput" type="number">
        </Transition>
        <button class="mx-2 rounded bg-yellow-500 center text-slate-900 material-symbols-outlined" @click="addCoin">Add</button>
    </div>
</template>

<script setup lang="ts">
    const coins = useState<number>('coins');
    const isShowInput = ref(false);
    const inputCoin = ref('');
    function addCoin() {
        if (!isShowInput.value) {
            isShowInput.value = true;
            return;
        }

        if (inputCoin.value === '' || parseInt(inputCoin.value) <= 0) {
            isShowInput.value = false;
            return;
        }

        coins.value = coins.value + parseInt(inputCoin.value);
        inputCoin.value = '';
        isShowInput.value = false;
    }
</script>

<style>
    .input-fade-enter-active {
        transition: all 0.3s ease-out;
    }

    .input-fade-leave-active {
        transition: all 0.1s ease-out;
    }

    .input-fade-enter-from,
    .input-fade-leave-to {
        width: 0;
    }
</style>