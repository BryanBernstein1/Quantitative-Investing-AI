# Quantitative-Investing-AI
Machine learning for predictive market patterns

use machine learning algorithms to analyze large amounts of market data, such as price and volume data, news articles, and social media sentiment, among others. This data could be used to train predictive models that generate trading signals based on historical patterns and trends.

This code collects market data from various sources, such as price and volume data, news articles, and social media sentiment, and have preprocessed the data to create a clean dataset with a binary target variable indicating whether the next day's return is positive or negative.

The code then extracts a set of features from the market data, such as the closing price, volume, and sentiment scores, and splits the data into training and testing sets.

A random forest classifier is then trained on the training data using the RandomForestClassifier class from the sklearn.ensemble module. The model is used to generate trading signals based on the predicted returns for the test data, and the model accuracy is evaluated using the accuracy_score function from the sklearn.metrics module. Based on the data gathered the AI will buy and sell from one fund to reach a goal of increasing the fund using quantitative investing. 
