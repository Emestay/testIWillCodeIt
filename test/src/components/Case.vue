<template>
    <div :class="caseClass" class="w-32 h-32 flex items-center justify-center text-lg font-bold border-solid border-4 border-white">
        {{ value }}
    </div>
</template>

<script>
export default {
    props: ['value'],
    computed: {
        caseClass() {
            return {
                'bg-red-200': this.value === 2,
                'bg-red-300': this.value === 4,
                'bg-red-400': this.value === 8,
                'bg-red-500': this.value === 16,
                'bg-red-600': this.value === 32,
                'bg-yellow-400': this.value === 64,
                'bg-yellow-500': this.value === 128,
                'bg-yellow-600': this.value === 256,
                'bg-yellow-700': this.value === 512,
                'bg-yellow-800': this.value === 1024,
                'bg-yellow-900': this.value === 2048,
            };
        }
    }
};
</script>

