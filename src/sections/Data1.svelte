<script>
    import Scroller from "../lib/Scroller.svelte";
    import ArticleText from "../lib/ArticleText.svelte";
  import App from "../App.svelte";
</script>

<section class="gradient-buffer"></section>

<div>
    <Scroller layout="left">
        {#snippet sticky()}
            <img src="./src/BWDCGraph1.png" alt="White median total net worth for households is at $284K while Black is 44K." width=800px>
        {/snippet}
        {#snippet scrolly()}
            <ArticleText>
                Today, Overtown's median income remains far below the Miami-Dade average.
                <a href="https://maps.fiu.edu/mpotop/sites/default/files/community_reports/Overtown%20CBR%20Final%20Draft.pdf">In 2010, the average income per capita was $11,578, which is much lower than Miami-Dade County's average per capita income of $28,823.</a>
            </ArticleText>
            <ArticleText>
                This brings attention to the greater concern as nationally, Black median household wealth today is just 15% of white households.
            </ArticleText>
            <ArticleText>
                Overtown’s story isn’t just about buildings and highways, it's about the people whose lives depended on access to education, jobs, homes, and financial security. Each of these threads weaves together to shape the wealth and health of a community.
            </ArticleText>
            <ArticleText>
                When schools close, or affordable homes vanish, or jobs disappear, the impact ripples far beyond the immediate loss and touches families, businesses, and futures.
            </ArticleText>
            <ArticleText>
                Understanding these connections can be overwhelming, but they are vital to see clearly if we want to build better neighborhoods and protect the resources that allow them to prosper.
            </ArticleText>

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

  .bike {
    transform: scaleX(-1);
  }
</style>