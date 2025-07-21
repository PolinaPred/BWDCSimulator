<script>

    let town = [];
    let stats = {
        population: 1000,
        averageWealth: 20000,
        employmentRate: 0.6,
        homeownershipRate: 0.4,
    };

    const buildings = [
        {
            name: 'school',
            effect: () => {
                stats.employmentRate += 0.05;
                stats.averageWealth += 2000;
            },
        },
        {
            name: 'college',
            effect: () => {
                stats.employmentRate += 0.1;
                stats.averageWealth += 5000;
                stats.homeownershipRate += 0.1;
            },
        },
        {
            name: 'hospital',
            effect: () => {
                stats.employmentRate += 0.02;
            },
        },
        {
            name: 'house',
            effect: () => {
                stats.homeownershipRate += 0.05;
            },
        },
        {
            name: 'office',
            effect: () => {
                stats.employmentRate += 0.08;
                stats.averageWealth += 3000;
            },
        },        
    ];

    function addBuilding(building){
        town.push(building);
        building.effect();
    }
</script>

<div class="simulator-wrapper">
    <h1>Build Your Neighorhood</h1>
    <p class="intro">Drag buildings into your town to see how they impact citizen wealth, employment, and homeownership.</p>

    <div class="container">
        <div class="sidebar">
            <h3>Build</h3>
            {#each buildings as building}
            <div
                class="building"
                on:click={() => addBuilding(building)}
                title={building.name}
            >
                    {building.name}
            </div>
            {/each}
            <div class="stats">
                <h4> Town Stats</h4>
                <p>Population: {stats.population}</p>
                <p>Average Wealth: ${stats.averageWealth.toLocaleString()}</p>
                <p>Employment: {(stats.employmentRate * 100).toFixed(1)}%</p>
                <p>Homeownership: {(stats.homeownershipRate * 100).toFixed(1)}%</p>
            </div>
        </div>

        <div class="town">
            {#if town.length === 0}
                <div class="empty-town">Add buildings to grow your neighborhood!</div>
                {/if}
                {#each town as building}
                    <div class="building">{building.name}</div>
                {/each}
        </div>
    </div>
</div>