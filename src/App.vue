<template>
    <header>
        <img :src="headerImg" alt="" />
        <h1>PokeGuía</h1>
    </header>

    <form @submit.prevent="pokeFetch(pokeInp)" action="">
        <label for="pokeNombre">Nombre: </label>
        <input
            v-model="pokeInp"
            type="text"
            name="pokeNombre"
            id="pokeNombre"
        />
        <button type="submit">Buscar</button>
        <p v-if="numbersInInput">Por favor, ingrese solo el nombre del pokemon</p>
    </form>

    <main v-if="data">
        <img :src="img" :alt="data.name" />

        <ul>
            <h2>Movimientos</h2>
            <li v-for="(move, index) in moves" :key="'move' + index"> {{move.move.name}} </li>
        </ul>
        <ul>
            <h2>Habilidades</h2>
            <li v-for="(ability, index) in abilities" :key="'ability' + index"> {{ability.ability.name}} </li>
        </ul>
    </main>
</template>

<script>
export default {
    data() {
        return {
            pokeInp: "",
            headerImg: `https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/International_PokÃ©mon_logo.svg/1920px-International_PokÃ©mon_logo.svg.png`,
            data: undefined,
            numbersInInput: false,
        };
    },
    methods: {
        async pokeFetch(val) {
            try {
                if (/^\d$/.test(this.pokeInp)) {
                    this.data = undefined;
                    return this.numbersInInput = true;
                }
                const res = await fetch(`https://pokeapi.co/api/v2/pokemon/${val}`);
                const resJson = await res.json();
                if (!res.ok)
                    return this.catch("Fuck");

                // this.img = resJson.sprites.front_default
                // resJson.moves.forEach(item => {
                // 	this.moves.push(item.move.name)
                // });
                this.numbersInInput = false;
                this.data = resJson;
                console.log(resJson);
                this.pokeInp = "";
            }
            catch (err) {
                console.error(err);
            }
        },
    },
    created() {
        this.pokeFetch('pikachu');
    },
    computed: {
        img() {
            return this.data.sprites.front_default;
        },
        moves() {
            return this.data.moves;
        },
        abilities() {
            return this.data.abilities;
        },
    },
};
</script>

<style lang="scss">
header {
    width: fit-content;
    margin: 0 auto;
    text-align: center;

    img {
        width: clamp(20em, 50vw, 72em);
    }
}

form {
    width: fit-content;
    margin: 0 auto;
}

main {
    width: fit-content;
    margin: 0 auto;
    text-align: center;
}
</style>
