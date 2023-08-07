<template>
    <div class="grid grid-cols-4 gap-4 p-4 bg-gray-200 border-solid border-2 border-slate-500">
        <Case v-for="(value, index) in gameCase" :key="index" :value="value" @keyPressed="keyPressed" />
    </div>
</template>
  
<script>
import Case from './Case.vue';

export default {
    components: {
        Case,
    },
    data() {
        return {
            gameCase: this.initgameCase(),
        };
    },
    methods: {
        // Cette méthode initialise le plateau de jeu avec 16 cases vides et ajoute deux tuiles de valeur 2
        initgameCase() {
            // Créer un tableau avec 16 cases, toutes initialisées à 0 (vide)
            const gameCase = Array(16).fill(0);

            // Ajouter une tuile avec la valeur 2 dans une case vide aléatoire
            this.addRandomTile(gameCase);
            // Faire la même chose une deuxième fois
            this.addRandomTile(gameCase);

            // Retourner le jeu
            return gameCase;
        },

        addRandomTile(gameCase) {

            let emptyIndex = [];

            // Parcourir toutes les cases
            for (let index = 0; index < gameCase.length; index++) {
                // Si la case est vide (valeur 0)
                if (gameCase[index] === 0) {
                    // je push sur emptyIndex
                    emptyIndex.push(index);
                }
            }

            // Si une case vide
            if (emptyIndex.length > 0) {
                // Choisir un indice aléatoire parmi les indices des cases vides
                let randomIndex = emptyIndex[Math.floor(Math.random() * emptyIndex.length)];
                gameCase[randomIndex] = 2; // Je met 2 de base dans la case vide choisie
            }
        },

        moveCase(gameCase, direction) {

            for (let index = 0; index < gameCase.length; index++) {

                const currentTile = gameCase[index];


                if (currentTile !== 0) {
                    let nextIndex = index;

                    switch (direction) {
                        case 'ArrowLeft':
                            nextIndex -= 1;
                            break;
                        case 'ArrowRight':
                            nextIndex += 1;
                            break;
                        case 'ArrowUp':
                            nextIndex -= 4;
                            break;
                        case 'ArrowDown':
                            nextIndex += 4;
                            break;
                    }


                    if (0 <= nextIndex && nextIndex < gameCase.length) {
                        // Si la case de destination n'est pas vide et que les valeurs des deux cases sont égales, on fusionne les deux cases
                        if (gameCase[nextIndex] !== 0 && gameCase[nextIndex] === currentTile) {
                            gameCase[index] = 0;
                            gameCase[nextIndex] *= 2;

                        } else {
                            gameCase[index] = 0;
                            gameCase[nextIndex] = currentTile;
                        }
                    }
                }
            }
        },
        //touches directionelle
        keyPressed(e) {
            switch (e.key) {
                case "ArrowLeft":
                    this.moveCase(this.gameCase, "ArrowLeft");
                    break;
                case "ArrowRight":
                    this.moveCase(this.gameCase, "ArrowRight");
                    break;
                case "ArrowUp":
                    this.moveCase(this.gameCase, "ArrowUp");
                    break;
                case "ArrowDown":
                    this.moveCase(this.gameCase, "ArrowDown");
                    break;
                default:
                    break;
            }
            //si une case a 0 restante logique de fin de game
            if (this.gameCase.some(tile => tile === 0)) {
                this.addRandomTile(this.gameCase);
            }
        }
    },
    mounted() {
        // Ajoute un listener pour les touches pressées dès que le composant est monté
        window.addEventListener('keydown', this.keyPressed);
    },

}

</script>