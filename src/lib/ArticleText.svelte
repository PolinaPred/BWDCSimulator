<script>
    let { children } = $props();
</script>

<div class="article-text">
    <p>
        {@render children()}
    </p>
</div>

<style>
    .article-text {
        margin: 50vh auto;
        width: 50%;
        background-color: #5a9e4d;
        color: #ffc845;
        border: solid #ffc845 3px;
        border-radius: 20px;
        padding: 20px;
        box-shadow: 16px 16px #ff914d;
        font-size: 1.2rem;
    }
</style>
