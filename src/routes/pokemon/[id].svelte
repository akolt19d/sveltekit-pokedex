<script context="module">
    export async function load({params}) {
        const id = params.id;
        const url = `https://pokeapi.co/api/v2/pokemon/${id}`
        const res = await fetch(url)
        const pokeman = await res.json()
        return {props: {pokeman}}
    }
</script>
<script>
    export let pokeman;
    let types = []
    pokeman.types.forEach(type => {
        types[types.length] = type.type.name
    });

    let sprites = []
    for(const [sprite, value] of Object.entries(pokeman.sprites)) {
        if(value != null && typeof value === "string")
            sprites[sprites.length] = value
    };
    let activeSprite = sprites[sprites.length - 1]

    $: {
        function loop() {
            for(let i = 0; i < sprites.length; i++)
            {
                setTimeout(() => {
                    activeSprite = sprites[i]
                    if(i == sprites.length - 1)
                        loop()
                }, (i+1) * 1000);
            }
        }
        loop()
    }

</script>

<div class="flex flex-col items-center">
    <h1 class="text-4xl text-center my-8 uppercase">{pokeman.name}</h1>
    <p>Type: <strong>{types}</strong> | Height: <strong>{(pokeman.height * 0.1).toFixed(1)}m</strong> | Weight: <strong>{(pokeman.weight * 0.1).toFixed(1)}kg</strong></p>
    <img class="card-image w-56 h-56" src={activeSprite} alt={`This is an image of ${pokeman.name}`}>
</div>