<template>
    <div class="stepper-container">
        <div class="step-wrapper" v-for="step in totalSteps" :key="step">
            <div class="step" :class="{ active: stepperNumber >= step }">{{ step }}</div>
            <div class="line" :class="{ active: stepperNumber > step }" v-if="step < totalSteps"></div>
        </div>
    </div>
</template>
<script>

export default {
    name: 'stepper-container',
    props: {
        stepperNumber: {
            type: Number,
            required: true
        },
        totalSteps: {
            type: Number,
            required: true
        },
    },
    data: () => ({

    }),
}
</script>

<style scoped>
.stepper-container {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.step-wrapper {
    display: flex;
    align-items: center;
}

.step {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 30px;
    height: 30px;
    border-radius: 50%;
    background-color: #374151;
}

.step.active {
    background-color: var(--primary-color);
    color: black;
}

.line {
    width: 60px;
    height: 2px;
    background-color: #374151;

    @media (max-width: 900px) {
        width: 100px;
    }
}

.line.active {
    background-color: var(--primary-color);
}
</style>