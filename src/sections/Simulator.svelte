<script>

    let town = [];
    town = town.filter(b => typeof b === 'object' && b.name);

    let govBudget = 1000000;
    let taxRate = 0.1;
    let priceCoefficient = 1;
    let taxes = 0;

    $: govBudget += stats.averageWealth * taxRate;
    $: totalBuildings = town.length;

    let stats = {
        population: 10,
        averageWealth: 0,
        employmentRate: 0,
        homeownershipRate: 0,
        debtConst: 1000,
        avgDebt: 0,
        businessMax: 1,
        bankMultiplier: 1,
        taxes: 0,
    };

    const buildings = [
        {
            name: '🏥',
            id: 'hospital',
            price: 100000 * priceCoefficient,
            effect: () => {
                stats.employmentRate += 0.01;
                stats.population += 20;
            },
            effectUndo: () => {
                stats.employmentRate -= 0.01;
                stats.population -= 20;
            }
        },
        {
            name: '🏫',
            id: 'school',
            price: 75000 * priceCoefficient,
            effect: () => {
                stats.employmentRate += 0.1;
            },
            effectUndo: () => {
                stats.employmentRate -= 0.1;
            }
        },
        {
            name: '🎓',
            id: 'college',
            price: 180000 * priceCoefficient,
            effect: () => {
                stats.employmentRate += 0.3;
                stats.businessMax += 3;
                stats.avgDebt += stats.debtConst * (1+stats.bankMultiplier);
                stats.bankMultiplier += 0.1;
            },
            effectUndo: () => {
                stats.employmentRate -= 0.3;
                stats.businessMax -=0;
                stats.avgDebt -= stats.debtConst * (1+stats.bankMultiplier);
                stats.bankMultiplier -= 0.1;
            }
        },
        {
            name: '🏘️',
            id: 'home',
            price: 90000 * priceCoefficient,
            effect: () => {
                stats.homeownershipRate += 0.02;
                stats.avgDebt += (stats.debtConst * 0.5 * (1+stats.bankMultiplier));
                stats.bankMultiplier += 0.05;
            },
            effectUndo: () => {
                stats.homeownershipRate -= 0.02;
                stats.avgDebt -= (stats.debtConst * 0.5 * (1+stats.bankMultiplier));
                stats.bankMultiplier -= 0.05;
            }
        },
        {
            name: '💼',
            id: 'business',
            price: 110000 * priceCoefficient,
            effect: () => {
                stats.employmentRate += 0.4;
            },
            effectUndo: () => {
                stats.employmentRate -= 0.4;
            }
        },
        {
            name: '💰',
            id: 'bank',
            price: 250000 * priceCoefficient,
            effect: () => {
                stats.bankMultiplier -= 0.3;
                stats.homeownershipRate += 0.01;
            },
            effectUndo: () => {
                stats.bankMultiplier +- 0.3;
                stats.homeownershipRate -= 0.01;
            }
        }        
    ];

    function addBuilding(name){
        const building = buildings.find(b => b.name === name || b.id === name);
        
        if (!building) return;

        const currentOffices = town.filter (b => b.name === '💼').length;
        if(building.name === '💼' && currentOffices >= stats.businessMax) {
            alert("You've reached the maximum number of offices allowed! You need to train more workers!");
            return;
        }

        
        if (building){
            if(govBudget >= building.price){
                govBudget -= building.price;
                town = [...town, {...building}];
                building.effect();

                stats.averageWealth = (totalBuildings * 1000) * stats.employmentRate - (taxRate * stats.averageWealth);
                taxes = taxRate * stats.averageWealth * stats.population;
                govBudget += taxes;
                priceCoefficient = 1 + Math.log10(stats.population+1) * 0.2;
                stats.population -= stats.population * (1-taxRate);
                stats.homeownershipRate -= taxRate/5;
                stats.employmentRate -= stats.population/100;

                stats = {...stats};
            }else{
                alert("Not enough budget to build that!");
            }
        }
    }

    function handleDrop(event) {
        event.preventDefault();
        const name = event.dataTransfer.getData('text/plain');
        const building = buildings.find((b) => b.name === name);
        if(name){
            addBuilding(name);
        }
    }
    
    function removeBuilding(i){
        if(!Array.isArray(town)) town = [];
        const removed = town[i];
        const base = buildings.find(b => b.name === removed.name || b.id === removed.id);
        if (typeof removed !== 'object' || !removed.name){
            console.warn("Invalid building found in town:", removed);
            town = town.filter((_, index) => index !== i);
            return;
        }

        govBudget += removed.price * 0.2;
        removed.effectUndo?.();
        town = town.filter((_, index) => index !== i);

        taxes = taxRate * stats.averageWealth * stats.population;
        govBudget += taxes;
        stats = {...stats};
    }

</script>

<div class="simulator-wrapper">
    <h1>Build Your Neighorhood</h1>
    <p class="intro">Drag buildings into your town to see how they impact citizen wealth, employment, and homeownership.</p>

    <div class="container">
        <div class="sidebar">

            <div class="stats">
                <h4> Town Stats</h4>
                <p><strong> Budget:</strong> ${govBudget.toFixed(2)}</p>
                <p>Population: {stats.population}</p>
                <p>Average Wealth: ${stats.averageWealth.toLocaleString()}</p>
                <p>Employment: {(stats.employmentRate * 100).toFixed(1)}%</p>
                <p>Homeownership: {(stats.homeownershipRate * 100).toFixed(1)}%</p>
                <p>Average Debt: ${stats.avgDebt}</p>
            </div>

            <div class="tax-slider">
                <label for="taxRate">Tax Rate: {(taxRate).toFixed(0)}%</label>
                <input
                    type="range"
                    id="taxRateSlider"
                    min="0"
                    max="100"
                    step="0.01"
                    bind:value={taxRate}
                />
            </div>

            <h3>Build</h3>
            {#each buildings as building}
            <div
                class="building {building.name === 'office' && town.filter(b => b.name === 'office').length >= stats.businessMax ? 'disabled' : ''}"
                title={building.name}
                draggable={!(building.name === 'office' && town.filter(b => b.name === 'office').length >= stats.businessMax)}
                on:dragstart={(e) => e.dataTransfer.setData('text/plain', building.name)}
                >
                    {building.name}
                    <span class="price">${building.price}</span>
            </div>
            {/each}
            
        </div>

        <div 
            class="town"
            on:dragover|preventDefault
            on:drop={handleDrop}>
            {#if town.length === 0}
                <div class="empty-town">Add buildings to grow your neighborhood!</div>
                {/if}
                {#each town as building, i}
                    <div class="building instance"
                    on:click={() => removeBuilding(i)}>
                    {building.name}
                </div>
                {/each}
        </div>
    </div>
</div>