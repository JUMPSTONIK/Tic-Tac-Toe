<template>
    <div 
        class="box"
        :class="turn == 'X' ? 'xTurn' : 'oTurn'" 
        @click="handleClick"   
    >
        <div v-if="turn.length" class="text">
            {{ turn }}
        </div>
    </div>
</template>

<script setup>
import { computed} from 'vue';

const {turn, winner} = defineProps({
    turn: {
        type: String,
        default: ''
    },
    winner:{
        type: String,
        default: null
    }
})

const emit = defineEmits(['setSymbol'])

const handleClick = () =>{
    emit('setSymbol')
}

</script>

<style scoped>
.box{
    @apply p-8 border border-white w-32 h-32 flex justify-center items-center;
}

.xTurn{
    @apply text-green-500
}

.oTurn{
    @apply text-orange-500
}

.text{
    @apply text-7xl
}
</style>