<script setup lang="ts">
defineProps<{
    image: string;
    status: 'closed' | 'opened' | 'matched';
    disabled?: boolean;
}>();
</script>

<template>
    <div
        class="card"
        :class="{
            'flipped': status !== 'closed',
            'matched': status === 'matched',
            'disabled': disabled
        }"
    >
        <div class="card-inner">
            <div class="card-front"></div>
            <div class="card-back">
                <span>{{ image }}</span>
            </div>
        </div>
    </div>
</template>

<style scoped>
.card {
    width: 100px;
    height: 100px;
    perspective: 1000px;
    cursor: pointer;
    user-select: none;
    margin-top: 10px;
    margin-bottom: 10px;
}

.card-inner {
    width: 100%;
    height: 100%;
    position: relative;
    transform-style: preserve-3d;
    transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
}


.card-front {
    background: linear-gradient(135deg, #5c758d 0%, #3a4e64 100%);
}


.card.flipped .card-back {
    background: white;
    color: #333; 
}


.card.matched .card-back {
    background: linear-gradient(135deg, #42b983 0%, #2c8a63 100%);
    color: white;
}


.card.flipped .card-inner {
    transform: rotateY(180deg);
}

.card-front,
.card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 8px;
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16);
}

.card-front {
    transform: rotateY(0deg);
}

.card-back {
    transform: rotateY(180deg);
    padding: 10px;
    box-sizing: border-box;
}

.card-image {
    max-width: 100%;
    max-height: 100%;
    object-fit: contain;
}

.card.disabled {
    cursor: not-allowed;
    opacity: 0.7;
}
</style>