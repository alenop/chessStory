<script setup>
import { TheChessboard } from 'vue3-chessboard';
import 'vue3-chessboard/style.css';
import { ref, onMounted, onBeforeUnmount,computed } from 'vue';

let boardAPI;
let o = {
  position: "start",
  moves: [
    { move: 'e2e4', message: "The beginning of the life" },
    { move: 'e7e5', message: "The life answer normally" },
    { move: 'g1f3', message: "so i prepare myself to what's coming next" },
    { move: 'b8c6', message: 'the life prepare too' },
    { move: 'f1c4', message: 'then i give a preemptive attack' },
    { move: 'g8f6', message: 'the life protect itself' },
    { move: 'd2d3', message: 'i am consodilating myself' },
    { move: 'f8c5', message: 'then the life attack !' },
    { move: 'c1e3', message: 'i am protecting to the best i can' },
    { move: 'd7d6', message: 'the life show me some peace time' },
    { move: 'e1g1', message: 'i am retrating to my fort to meditate about my actions' },
    { move: 'e8e7', message: "the life is troubled and present me a opportunity" },
    { move: 'b1c3', message: 'i prepare myself more to be sure it works this time' },
    { move: 'b7b6', message: 'the life consolidate his attack' },
    { move: 'e3g5', message: 'i begin to prepare to the opportunity' },
    { move: 'c8b7', message: 'the life prepare to attack again' },
    { move: 'c3d5', message: 'then seeing myself a opportunity i attack !' },
    { move: 'e7f8', message: "the life retreat doesn't wanna give it to me easy" },
    { move: 'd5f6', message: 'I force the life to back down as scheduled' },
    { move: 'g7f6', message: 'the life take what she can' },
    { move: 'g5h4', message: 'I prepare myself to the final assault' },
    { move: 'c6d4', message: "the life doesn't like it prepare to attack" },
    { move: 'd1d2', message: 'I finish the preparations of attack' },
    { move: 'a7a5', message: 'life attack to the long term' },
    { move: 'g1h1', message: 'i defend myself before i have no other choice' },
    { move: 'd4f3', message: "the life still attack because that's still a good move" },
    { move: 'd2h6', message: "but i ignore the attack to focus on what's important" },
    { move: 'f8e7', message: 'the opportunity/reward try to escape' },
    { move: 'h6f6', message: "i don't let it go" },
    { move: 'e7e8', message: "the life doesn't have much of a choice" },
    { move: 'f6f7', message: 'the final the life will give me what i want' },
  ],
  currentMoveIndex: 0,
};



const messages = ref([]);
const time = 1000;
const paused = ref(false); // Add paused state

function mov(){
  if (!paused.value && o.currentMoveIndex < o.moves.length) {
    const { move, message } = o.moves[o.currentMoveIndex];
    messages.value.push(`Move ${o.currentMoveIndex + 1}: ${message}`);
    
    // Use the board API to make the move
    boardAPI.move({
      from: move.slice(0, 2),
      to: move.slice(2, 4),
    });
    
    o.currentMoveIndex++;
    setTimeout(mov, time);
  } else if (o.currentMoveIndex >= o.moves.length) {
    messages.value.push('Game Over');
  }
}

function handleBoardCreated(boardApi) {
  boardAPI = boardApi;
  setTimeout(mov, time);
}

function togglePause() {
  paused.value = !paused.value;
  if (!paused.value) {
    mov(); // Resume the game if it was paused
  }
}

function handleKeyPress(event) {
  if (event.key === 'p') { // Press 'p' to pause/resume
    togglePause();
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleKeyPress);
});

onBeforeUnmount(() => {
  window.removeEventListener('keydown', handleKeyPress);
});
const columns = computed(() => {
  const messagesPerColumn = 10; // Adjust this number to set how many messages per column
  const columnsArray = [];
  for (let i = 0; i < messages.value.length; i += messagesPerColumn) {
    columnsArray.push(messages.value.slice(i, i + messagesPerColumn));
  }
  return columnsArray;
});
</script>

<template>
  <div style='display:flex; flex-direction:row'>
    <TheChessboard
      @board-created="handleBoardCreated"
      :player-color="'white'"
    />
    <div style='display:flex; flex-direction:row'>
        <div v-for="(column, colIndex) in columns" :key="colIndex" style="margin: 0 10px;">
            <div style='display:flex; flex-direction:column'>
            <div v-for="(message, index) in column" :key="index" class="message">{{ message }}</div>
        </div>
    </div>
    </div>
  </div>
</template>

<style scoped>
.message {
  margin-top: 10px;
  font-size: 18px; /* Increase the font size */
  font-weight: bold; /* Make the text bold */
  color: #2c3e50; /* Set a dark color for the text */
  background-color: #f0f0f0; /* Light gray background */
  padding: 10px; /* Add some padding */
  border-radius: 5px; /* Rounded corners */
  border: 1px solid #ccc; /* Light border */
  width: fit-content; /* Fit the content width */
  margin: 10px auto; /* Center the messages */
}
</style>
