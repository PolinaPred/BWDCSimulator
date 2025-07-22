<script>
    import Scroller from "../lib/Scroller.svelte";
    import ArticleText from "../lib/ArticleText.svelte";
  import App from "../App.svelte";
</script>

<section class="gradient-buffer"></section>

<div>
    <Scroller layout="right">
        {#snippet scrolly()}
            <ArticleText>
                In the early 20th century, Overtown was a vibrant, self-sustaining Black community in Miami. 
            </ArticleText>
            <ArticleText>
                Segregation laws prevented Balck residents from living elsewhere, but inside Overtown, culture flourished.
            </ArticleText>
            <ArticleText>
                Churches, jazz clubs, and Black-owned businesses filled the streets. 
            </ArticleText>
            <ArticleText>
                Overtown was alive with resilience, despite the walls around it.
            </ArticleText>
        {/snippet}

        {#snippet sticky()}
            <img src="./src/BikeRider.jpg" alt="DJ Jockey Jack Gibson (left) posing for a promotional picture with Ruth Brown (center), and boxing champion Joe Louis (right)" width=800px>
        {/snippet}
    </Scroller>
</div>

<style>
    .chart {
        width: 90%;
        margin: 0px auto;
    }

    .gradient-buffer{
    height: 20vh;
    background:linear-gradient(to bottom, #0f0f0f 0%, #C8DFC4 100%);
  }
</style>
