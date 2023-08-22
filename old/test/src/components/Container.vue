<template>
    <div class="container mx-auto px-8 py-12 max-w-4xl bg-white shadow-2xl rounded-2xl">
        <h1 class="text-4xl font-bold text-gray-800 mb-8">2048</h1>
        <div class="grid grid-cols-4 p-6 bg-gray-100 rounded-xl shadow-md">
            <div v-for="row in gameCase" class="row">
                <Case v-for="(value, index) in row" :key="index" :value="value" />
            </div>
        </div>
        <div class="buttons mt-8 flex justify-center space-x-6">
            <button @click="startGame"
                class="start-btn bg-gradient-to-r from-green-400 to-green-600 hover:from-green-500 hover:to-green-700 text-white text-2xl font-extrabold py-3 px-8 rounded-xl transition-all ease-in-out duration-300 transform hover:scale-105 shadow-lg">
                Start
            </button>
            <button @click="abandonneLache"
                class="reset-btn bg-gradient-to-r from-red-400 to-red-600 hover:from-red-500 hover:to-red-700 text-white text-xl font-extrabold py-2 px-6 rounded-xl transition-all ease-in-out duration-300 transform hover:scale-105 shadow-lg">
                Reset
            </button>
        </div>
    </div>
</template>


<script>
import { ref } from 'vue';
import Case from './Case.vue';

export default {
    components: { Case },
    setup() {
        /**
         * @typedef {number[][]}
         */
        const gameCase = ref(initGame());

        function initGame() {
            // On crée notre plateau 4 par 4 tout est à 0 pour commencer
            return Array.from({ length: 4 }, () => Array(4).fill(0));
        }

        function startGame() {
            addRandomTile();
            addRandomTile();
            //On lance le jeu avec deux tuiles au hasard
        }

        function abandonneLache() {
            gameCase.value = initGame();
        }

        function addRandomTile() {
            //On ajoute une tuile au hasard
            const cellDispo = [];
            gameCase.value.forEach((row, rowIndex) => {
                row.forEach((cell, cellIndex) => {
                    if (cell === 0) {
                        cellDispo.push([rowIndex, cellIndex]);
                    }
                });
            });

            if (cellDispo.length > 0) {
                const [rowIndex, cellIndex] = cellDispo[Math.floor(Math.random() * cellDispo.length)];
                gameCase.value[rowIndex][cellIndex] = Math.random() < 0.9 ? 2 : 4;
            }
        }

        function slideRow(row) {
            // On fait glisser les tuiles dans la rangée

            const newRow = row.filter(val => val);
            return newRow.concat(Array(4 - newRow.length).fill(0));
        }

        function transpose(matrix) {
            //pour bosser sur les collones 
            return matrix[0].map((_, colIndex) => matrix.map(row => row[colIndex]));
        }


        // Source https://stackoverflow.com/questions/66201957/how-can-i-implement-the-merge-functionality-for-2048
        function mergeRow(row) {
            for (let i = 0; i < 3; i++) {
                for (let j = i + 1; j < 4; j++) {
                    if (row[i] === row[j] && row[i] !== 0) {
                        row[i] *= 2;
                        row[j] = 0;
                    }
                }
            }

            return row;
        }

        function moveRowLeft(row) {
            // test pour bouger tout à gauche
            let newRow = slideRow(row);
            newRow = mergeRow(newRow);
            newRow = slideRow(newRow);
            return newRow;
        }

        const moveRow = (numbers, dir = "left") => {
            const tmpNumbers = dir === "right" ? [...numbers].reverse() : [...numbers]
            const newRow = [0, 0, 0, 0]
            let emptyCellIdx = 0

            for (const [i, nb] of numbers.entries()) {

                console.log("___", emptyCellIdx, tmpNumbers, nb)
                if (nb === 0) {

                    continue;
                }

                if (emptyCellIdx - 1 >= 0 && newRow[emptyCellIdx - 1] === nb) {
                    newRow[emptyCellIdx - 1] = nb * 2

                } else {
                    newRow[emptyCellIdx] = nb
                    console.log("newRow", newRow)
                    emptyCellIdx = i
                }

                //tmpNumbers[i] = 0
            }

            return dir === "right" ? newRow.reverse() : newRow
        }

        const moveCol = (numbers, dir, = 'up') => {
            const tmpNumbers = dir = 'down' ? [...numbers].reverse() : [...numbers]


            for (let i = 0; i < 4; i++) {
                (array1[i], array2[i], array3[i], array4[i]);
            }
        }

        const slide = (direction) => {
            matrix
        }

        function keyPressed(event) {
            // listeners sur les touches fléché su clavier !
            let movedGameCase;
            if (event.key === 'ArrowLeft') {
                movedGameCase = gameCase.value.map(row => moveRowLeft(row));
                slide("left")
            }
            if (event.key === 'ArrowRight') {
                movedGameCase = gameCase.value.map(row => moveRowLeft(row.reverse()).reverse());
            }
            if (event.key === 'ArrowUp') {
                movedGameCase = transpose(gameCase.value).map(row => moveRowLeft(row));
                movedGameCase = transpose(movedGameCase);
            }
            if (event.key === 'ArrowDown') {
                movedGameCase = transpose(gameCase.value).map(row => moveRowLeft(row.reverse()).reverse());
                movedGameCase = transpose(movedGameCase);
            }

            if (movedGameCase) {
                gameCase.value = movedGameCase;
                addRandomTile();
            }




            //Fonction Win Loose
            // Mettre en SvelteKit pour co avec Supabase
            //Refaire en TS
            //Fonction leaderboard avec score, faire un store ! host sur supabase
            //Fonction if (newlyCreated === true){Merge impossible}
        }

        window.addEventListener('keydown', keyPressed);

        return {
            gameCase,
            startGame,
            abandonneLache,
        };
    },

};
</script>
