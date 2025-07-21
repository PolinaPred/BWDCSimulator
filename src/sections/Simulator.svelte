<script>

    let town = [];

    function handleDrop(event) {
        const name = event.dataTransfer.getData('text/plain');
        if(name){
            addBuilding(name);
        }
    }

    let stats = {
        population: 1000,
        averageWealth: 20000,
        employmentRate: 0.6,
        homeownershipRate: 0.4,
    };

    const buildings = [
        {
            name: '🏫',
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
            name: '🏥',
            effect: () => {
                stats.employmentRate += 0.02;
            },
        },
        {
            name: '🏘️',
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

    function addBuilding(name){
        const building = buildings.find(b => b.name === name || b.name.toLowerCase() === name.toLowerCase())
        if (building){
            town = [...town, building.name];
            building.effect();
            stats = {...stats};
        }
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
                draggable="true"
                on:dragstart={(e) => e.dataTransfer.setData('text/plain', building.name)}
                title={building.name}>
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

        <div 
            class="town"
            on:dragover|preventDefault
            on:drop={handleDrop}>
            {#if town.length === 0}
                <div class="empty-town">Add buildings to grow your neighborhood!</div>
                {/if}
                {#each town as building}
                    <div class="building">{building}</div>
                {/each}
        </div>
    </div>
</div>