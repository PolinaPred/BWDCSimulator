<script>
    import Scroller from "../lib/Scroller.svelte";
    import ArticleText from "../lib/ArticleText.svelte";
</script>

<div>
    <Scroller layout="left">
        {#snippet sticky()}
            <ArticleText>
                In the early 20th century, Overtown was a vibrant, self-sustaining Black community in Miami. Segregation laws prevented Black residents from living elsewhere, but inside Overtown, culture flourished.
            </ArticleText>

            <ArticleText>
                Churches, jazz clubs, and Black-owned businesses filled the streets. Overtown was alive with resilience, despite the walls around it.
            </ArticleText>

            <ArticleText>
                You might also want to add more interactivity or gamify parts of
                your scrollytelling piece.
            </ArticleText>
        {/snippet}

        {#snippet scrolly()}
            <div>
                <img src="./src/BikeRider.jpg" alt='DJ "Jockey Jack" Gibson (left) posing for a promotional picture with Ruth Brown (center), and boxing champion Joe Louis (right)''>
            </div> 
        {/snippet}
    </Scroller>
</div>