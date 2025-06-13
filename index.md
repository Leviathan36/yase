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
            <a href="rest_api_101/index">
                <h2>REST API 101</h2>
                <p class="subtitle">From design to implementation</p>
            </a>
        </div>
        <div class="article-image">
            <a href="rest_api_101/index">
                <img src="imgs/index/rest_api_101.png" alt="rest api 101">
            </a>
        </div>
    </div>

    <div class="article-card">
        <div class="article-content">
            <a href="malware_development/index">
                <h2>Malware development</h2>
                <p class="subtitle">Payload, dropper and C&C server</p>
            </a>
        </div>
        <div class="article-image">
            <a href="malware_development/index">
                <img src="imgs/index/malware_development.png" alt="Malware development">
            </a>
        </div>
    </div>
</div>

