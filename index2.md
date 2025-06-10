<style>
    /* Basic Reset & Body Styling */
    body {
        margin: 0;
        font-family: 'Roboto', sans-serif;
        background-color: #f8f8f8;
        color: #333;
        line-height: 1.6;
    }

    /* Container for the main content */
    .container {
        max-width: 700px; /* Medium-like content width */
        margin: 40px auto;
        padding: 0 20px;
    }

    /* Article Styling */
    .article-card {
        display: flex;
        margin-bottom: 40px;
        border-bottom: 1px solid #eee;
        padding-bottom: 30px;
        align-items: flex-start; /* Aligns items to the top of the flex container */
    }

    .article-content {
        flex-grow: 1;
        padding-right: 20px; /* Space between text and image */
    }

    .article-content h2 {
        font-family: 'Merriweather', serif;
        font-size: 1.8em;
        margin-top: 0;
        margin-bottom: 10px;
        line-height: 1.2;
    }

    .article-content h2 a {
        text-decoration: none;
        color: #333;
    }

    .article-content h2 a:hover {
        color: #007bff; /* Example hover color */
    }

    .article-content p.subtitle {
        font-size: 1.1em;
        color: #666;
        margin-bottom: 15px;
    }

    .article-image {
        flex-shrink: 0; /* Prevent image from shrinking */
        width: 200px; /* Fixed width for the cover image */
        height: 130px; /* Fixed height for the cover image */
        overflow: hidden; /* Ensures image doesn't exceed its container */
        border-radius: 4px; /* Slightly rounded corners for the image */
    }

    .article-image img {
        width: 100%;
        height: 100%;
        object-fit: cover; /* Crops the image to fit the container without distortion */
        display: block; /* Removes extra space below the image */
    }

    /* Responsive adjustments */
    @media (max-width: 768px) {
        .article-card {
            flex-direction: column-reverse; /* Puts image above text on smaller screens */
            align-items: center;
        }

        .article-content {
            padding-right: 0;
            text-align: center;
            margin-top: 20px;
        }

        .article-image {
            width: 100%; /* Image takes full width on small screens */
            height: 200px; /* Adjust height for better mobile view */
        }
    } 
</style>

<div class="container">

    <div class="article-card">
        <div class="article-content">
            <h2><a href="#">The Art of Mindful Living in a Busy World</a></h2>
            <p class="subtitle">Discover practical tips to slow down, be present, and find peace amidst the daily grind.</p>
        </div>
        <div class="article-image">
            <img src="https://placehold.co/600x400" alt="Mindful Living">
        </div>
    </div>

    <div class="article-card">
        <div class="article-content">
            <h2><a href="#">Understanding Quantum Physics: A Beginner's Guide</a></h2>
            <p class="subtitle">Demystifying the complex world of subatomic particles and their peculiar behaviors.</p>
        </div>
        <div class="article-image">
            <img src="https://placehold.co/600x400" alt="Quantum Physics">
        </div>
    </div>

    <div class="article-card">
        <div class="article-content">
            <h2><a href="#">The Future of Artificial Intelligence in Everyday Life</a></h2>
            <p class="subtitle">Exploring how AI is shaping our present and what to expect in the coming decades.</p>
        </div>
        <div class="article-image">
            <img src="https://placehold.co/600x400" alt="Future of AI">
        </div>
    </div>
</div>
