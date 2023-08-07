<template>
    <div class="grid grid-cols-4 gap-4 p-4 bg-gray-200">
        <Case v-for="(value, index) in gameCase" 
        :key="index" 
        :value="value" />
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
        }
    }

}

</script>