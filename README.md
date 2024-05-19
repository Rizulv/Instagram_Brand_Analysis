# Social Media Analysis for Major Mobile Brands

## Project Overview

This project focuses on analyzing the social media presence of major mobile brands—Apple, Samsung, and Google—on Instagram. The goal is to gather and analyze data such as the number of followers, likes, comments, and engagement metrics to understand the social media performance and user sentiment towards these brands. The project leverages Python, the `instagrapi` package, and visualization tools like Power BI to present the findings.

## Objectives

- **Data Collection**: Fetch data from Instagram for Apple, Samsung, and Google, including follower count, number of posts, likes, comments, and engagement metrics.
- **Sentiment Analysis**: Perform sentiment analysis on comments to determine user sentiment towards each brand.
- **Visualization**: Create visualizations to present the collected data and analysis results using Power BI.

## Installation

### Prerequisites

- Python 3.x
- `pip` package manager

### Dependencies

Install the required packages using pip:

```bash
pip install instagrapi pandas
```

## Project Structure

- `instagram_analysis.ipynb`: Jupyter notebook containing the code for data collection and analysis.
- `apple_historical_data.xlsx`, `samsung_historical_data.xlsx`, `google_historical_data.xlsx`: Excel files containing historical data for each brand.
- `instagram_data.xlsx`: Excel file where the collected Instagram data will be saved.
- `Instagram_Analysis_Documentation.docx`: Documentation detailing the project's setup, code explanation, and usage.

## Usage

### Data Collection

1. **Login to Instagram**: Use the `instagrapi` package to login to Instagram.

    ```python
    from instagrapi import Client

    def login_to_instagram(username, password):
        client = Client()
        client.login(username, password)
        return client
    ```

2. **Fetch Brand Data**: Fetch data for each brand including followers, following, total posts, top post likes, and comments.

    ```python
    def fetch_brand_data(client, username):
        account = client.user_info_by_username(username)
        followers = account.follower_count
        following = account.following_count
        total_posts = account.media_count
        medias = client.user_medias(account.pk, amount=1)
        top_post = medias[0]
        comments = client.media_comments(top_post.id, amount=20)
        comment_texts = [comment.text for comment in comments]
        return {
            "username": username,
            "followers": followers,
            "following": following,
            "total_posts": total_posts,
            "top_post_likes": top_post.like_count,
            "top_post_comments": top_post.comment_count,
            "comments": comment_texts
        }
    ```

3. **Save Data to Excel**: Save the fetched data to an Excel file.

    ```python
    def save_data_to_excel(data):
        df = pd.DataFrame.from_dict(data, orient='index').T
        df.to_excel('instagram_data.xlsx', index=False)
    ```

4. **Main Function**: Main function to execute the data collection process.

    ```python
    def main():
        username = 'your_instagram_username'
        password = 'your_instagram_password'
        client = login_to_instagram(username, password)
        brands = ['apple', 'samsung', 'google']
        data = {}
        for brand in brands:
            data[brand] = fetch_brand_data(client, brand)
        save_data_to_excel(data)

    if __name__ == "__main__":
        main()
    ```

### Sentiment Analysis

1. **Analyze Comments**: Perform sentiment analysis on the first 20 comments of the latest post for each brand.

    ```python
    from textblob import TextBlob

    def analyze_comments(comments):
        sentiments = [TextBlob(comment).sentiment.polarity for comment in comments]
        return sentiments
    ```

### Visualization

1. **Power BI**: Use Power BI to create visualizations of the collected data and analysis results. Visualize metrics such as follower growth, engagement rates, and sentiment scores.

## Results

- **Engagement Scores**: Calculate and compare engagement scores for Apple, Samsung, and Google.
- **Sentiment Analysis**: Visualize the sentiment distribution for each brand's comments.
- **Follower Trends**: Track and display the follower count trends over time.

## Conclusion

This project provides valuable insights into the social media performance of major mobile brands on Instagram. By analyzing followers, engagement metrics, and user sentiment, businesses can better understand their audience and tailor their social media strategies accordingly.

## Future Work

- **Expand Data Sources**: Include data from other social media platforms like Twitter and Facebook.
- **Deep Dive Analysis**: Perform deeper analysis on user demographics and post content.
- **Automate Visualization**: Automate the creation of visualizations using Power BI's Python integration.
