<script>

    let town = [];

    function handleDrop(event) {
        const name = event.dataTransfer.getData('text/plain');
        if(name){
            addBuilding(name);
        }
    }

    let stats = {
        population: 10,
        averageWealth: 0,
        employmentRate: 0,
        homeownershipRate: 0,
        govBudget: 1000000,
        debtConst: 1000,
        avgDebt: 0,
        businessMax: 1,
        bankMultiplier: 1
    };

    const buildings = [
        {
            name: '🏫',
            effect: () => {
                stats.employmentRate += 0.1;
            },
        },
        {
            name: '🎓',
            effect: () => {
                stats.employmentRate += 0.3;
                stats.businessMax += 3;
                stats.avgDebt += stats.debtConst * (1+stats.bankMultiplier);
                stats.bankMultiplier += 0.1;
            },
        },
        {
            name: '🏥',
            effect: () => {
                stats.employmentRate += 0.01;
                stats.population += 20;
            },
        },
        {
            name: '🏘️',
            effect: () => {
                stats.homeownershipRate += 0.02;
                stats.avgDebt += (stats.debtConst * 0.5 * (1+stats.bankMultiplier));
                stats.bankMultiplier += 0.05;
            },
        },
        {
            name: '💼',
            effect: () => {
                stats.employmentRate += 0.5;
            },
        },
        {
            name: '💰',
            effect: () => {
                stats.bankMultiplier -= 0.3;
                stats.homeownershipRate += 0.01;

            }
        }        
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