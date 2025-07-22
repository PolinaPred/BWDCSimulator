<script>
    let { title, subtitle } = $props();
</script>

<div class="title-card">
    <div class="bg-pattern"></div>
    <div class="content">
        <h1>{title}</h1>
        <p>{subtitle}</p>
    </div>
</div>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&family=Fredoka:wght@700&display=swap');

    .title-card {
        position: relative;
        background-color: #1e2b19;
        height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;
        padding: 2rem;
        box-sizing: border-box;
        overflow: hidden;
        font-family: "Inter", sans-serif;
    }

    .bg-pattern {
        position:absolute;
        top: -20%;
        left: -20%;
        width: 200%;
        height: 200%;
        background: radial-gradient(#5a9e4d55 2%, transparent 2%) 0 0 / 80px 80px;
        animation: float 30s linear infinite;
        opacity: 0.2;
        z-index: 0;
    }

    @keyframes float {
        from{
            transform: translate(0, 0);
        }
        to {
            transform: translate(-100px, -100px);
        }
    }

    .content {
        position: relative;
        max-width: 700px;
        background-color: #5a9e4d;
        padding: 2.5rem;
        border: 6px solid #ffc845;
        border-radius: 2rem;
        box-shadow: 16px 16px 0 #ff914d;
        transform: skewY(-2deg);
        transition: transform 0.3s ease, box-shadow 0.3s ease;
        z-index: 1;
    }

    .content:hover {
        transform: scale(1.02) skewY(-2deg);
        box-shadow: 16px 16px 0 #ff914d;
    }

    h1 {
        font-family: "Fredoka", sans-serif;
        font-size: 5rem;
        margin: 0;
        color: #ffc845;
        text-shadow: 2px 2px 0 #ff914d;
        letter-spacing: 1px;
    }

    p {
        font-size: 1.3rem;
        color: #f7f5eb;
        margin-top: 1.2rem;
        line-height: 1.6;
    }

    @media (max-width: 600px) {
        h1 {
            font-size: 2.2rem;
        }

        p {
            font-size: 1.1rem;
        }
    }
</style>
