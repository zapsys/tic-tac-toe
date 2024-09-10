<template>
    <v-container fill-height>
        <v-row class="justify-center">
            <v-container fluid>
                <v-card class="border-sm rounded-lg my-3 pa-3 elevation-4">
                    <h1 class="text-h2 text-center font-weight-bold my-2">Tic Tac Toe</h1>
                </v-card>
            </v-container>
        </v-row>
        <v-row class="mx-n2">
            <v-col class="align-content-center" cols="12" sm="3" md="3" lg="3" xl="3">
                <v-card class="border-sm rounded-lg pa-4 align-content-center elevation-4">
                    <h1 class="text-h1 font-weight-bold text-center text-success">X</h1>
                    <h1 class="text-center">{{ xCount }} {{ xCount == 1 ? 'Vitória' : 'Vitórias' }}</h1>
                    <v-card-actions class="justify-center">
                        <div v-if="currentPlayer === 'X'">
                            <v-btn variant="outlined">Sua Vez</v-btn>
                        </div>
                    </v-card-actions>
                </v-card>
            </v-col>
            <v-col cols="12" sm="6" md="6" lg="6" xl="6">
                <v-row class="align-center justify-center" no-gutters>
                    <v-col v-for="r in 3" :key="r" cols="3" id="board">
                        <v-sheet id="board-cell" v-text="board[r - 1][c - 1]" @click="turn(r - 1, c - 1)"
                            :class="[disable || board[r - 1][c - 1] !== '' ? 'click-event' : '', board[r - 1][c - 1] === 'X' ? 'text-success' : 'text-primary']"
                            v-for="c in 3" :key="c" class="text-h2 font-weight-bold my-2 rounded-lg" width="125px"
                            height="125px">
                        </v-sheet>
                    </v-col>
                </v-row>
            </v-col>
            <v-col class="align-content-center" cols="12" sm="3" md="3" lg="3" xl="3">
                <v-card class="border-sm rounded-lg align-content-center pa-4 elevation-4">
                    <h1 class="text-h1 font-weight-bold text-center text-primary">O</h1>
                    <h1 class="text-center">{{ yCount }} {{ yCount == 1 ? 'Vitória' : 'Vitórias' }}</h1>
                    <v-card-actions class="justify-center">
                        <div v-if="currentPlayer === 'O'">
                            <v-btn variant="outlined">Sua Vez</v-btn>
                        </div>
                    </v-card-actions>
                </v-card>
            </v-col>
        </v-row>
        <v-row class="justify-center">
            <v-container fluid>
                <v-card v-if="winner" class="border-sm rounded-lg my-3 pa-4 elevation-4">
                    <v-card-title>
                        <h1 class="text-center" v-if="winner !== 'Draw'">
                            Jogador : <span :class="winner === 'X' ? 'text-success' : 'text-primary'">
                                {{ winner }}</span>
                            venceu a partida!
                        </h1>
                        <h1 class="text-center" v-else>Partida terminou empatada</h1>
                    </v-card-title>
                    <v-card-actions class="justify-center mt-3">
                        <v-btn color="primary" variant="outlined" @click="nextGame">Próximo Jogo</v-btn>
                        <v-btn color="warning" variant="outlined" @click="completeReset">Reiniciar Jogo</v-btn>
                    </v-card-actions>
                </v-card>
            </v-container>
        </v-row>
    </v-container>
</template>

<script>
export default {
    name: 'Board',
    data: () => ({
        startPlayer: 'X',
        currentPlayer: 'X',
        xCount: 0,
        yCount: 0,
        board: [
            ['', '', ''],
            ['', '', ''],
            ['', '', '']
        ],
        winner: '',
        disable: false
    }),

    watch: {
        board: {
            handler: function () {
                const check = (x1, x2, y1, y2, z1, z2) => {
                    return (
                        this.board[x1][x2] !== '' &&
                        this.board[y1][y2] !== '' &&
                        this.board[z1][z2] !== '' &&
                        this.board[x1][x2] === this.board[y1][y2] &&
                        this.board[y1][y2] === this.board[z1][z2]
                    )
                }
                const exists = (arr) => {
                    return arr.some((row) => row.includes(''))
                }

                const conditionArray = [
                    check(0, 0, 0, 1, 0, 2),
                    check(1, 0, 1, 1, 1, 2),
                    check(2, 0, 2, 1, 2, 2),
                    check(0, 0, 1, 0, 2, 0),
                    check(0, 1, 1, 1, 2, 1),
                    check(0, 2, 1, 2, 2, 2),
                    check(0, 0, 1, 1, 2, 2),
                    check(0, 2, 1, 1, 2, 0)
                ]

                if (conditionArray.includes(true)) {
                    this.winner = this.currentPlayer === 'X' ? 'O' : 'X'
                    this.disable = true
                    this.currentPlayer = 'X'
                    if (this.winner === 'X') this.xCount++
                    else if (this.winner === 'O') this.yCount++
                } else if (!exists(this.board)) {
                    this.currentPlayer = 'Z'
                    this.winner = 'Draw'
                }
            },
            deep: true
        }
    },
    methods: {
        turn(x, y) {
            if (this.currentPlayer === 'X') {
                this.board[x][y] = this.currentPlayer
                this.currentPlayer = 'O'
            } else {
                this.board[x][y] = this.currentPlayer
                this.currentPlayer = 'X'
            }
        },
        nextGame() {
            this.disable = false
            this.board = [
                ['', '', ''],
                ['', '', ''],
                ['', '', '']
            ]
            this.winner = ''
            if (this.startPlayer === 'X') {
                this.currentPlayer = 'O'
                this.startPlayer = 'O'
            } else {
                this.currentPlayer = 'X'
                this.startPlayer = 'X'
            }
        },
        completeReset() {
            this.disable = false
            this.board = [
                ['', '', ''],
                ['', '', ''],
                ['', '', '']
            ]
            this.winner = ''
            this.xCount = 0
            this.yCount = 0
            this.currentPlayer = 'X'
            this.startPlayer = 'X'
        }
    }
}
</script>
<style>
#board {
    max-width: 132px;
}

#board-cell {
    display: flex;
    border: 2px solid black;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: all 0.2s ease-in-out;
}

#board-cell:hover {
    transform: scale(1.025);
    border: 3px solid orangered;
}

.click-event {
    pointer-events: none;
}

@media (min-width: 660px) and (max-width: 1024px) {
    #board-cell {
        width: 100px !important;
        height: 100px !important;
    }
}
@media (max-width: 659px) {
    #board-cell {
        width: 85px !important;
        height: 85px !important;
    }
}
</style>