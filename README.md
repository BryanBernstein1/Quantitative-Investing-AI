
import java.time.LocalDate;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;

import org.apache.commons.math3.stat.descriptive.moment.StandardDeviation;
import org.apache.commons.math3.stat.regression.OLSMultipleLinearRegression;
import org.apache.commons.math3.stat.regression.SimpleRegression;
import org.apache.commons.math3.util.Pair;
import org.json.JSONArray;
import org.json.JSONObject;

public class MarketDataAnalyzer {
    
    private static final String NEWS_API_KEY = "your_api_key_here";
    
    public static void main(String[] args) {
        String ticker = "AAPL";
        LocalDate startDate = LocalDate.of(2010, 1, 1);
        LocalDate endDate = LocalDate.now();
        MarketData data = getMarketData(ticker, startDate, endDate);
        System.out.println(data.toString());
    }
    
    public static MarketData getMarketData(String ticker, LocalDate startDate, LocalDate endDate) {
        // Collect market data using Yahoo Finance API
        String url = String.format("https://query1.finance.yahoo.com/v7/finance/download/%s?period1=%d&period2=%d&interval=1d&events=history",
                                   ticker, startDate.toEpochDay(), endDate.toEpochDay());
        List<String[]> rows = HttpUtils.getCsvData(url);
        
        // Clean and preprocess data
        List<LocalDate> dates = new ArrayList<>();
        List<Double> opens = new ArrayList<>();
        List<Double> highs = new ArrayList<>();
        List<Double> lows = new ArrayList<>();
        List<Double> closes = new ArrayList<>();
        List<Double> adjCloses = new ArrayList<>();
        List<Double> volumes = new ArrayList<>();
        for (int i = 1; i < rows.size(); i++) {
            String[] row = rows.get(i);
            LocalDate date = LocalDate.parse(row[0]);
            Double open = Double.parseDouble(row[1]);
            Double high = Double.parseDouble(row[2]);
            Double low = Double.parseDouble(row[3]);
            Double close = Double.parseDouble(row[4]);
            Double adjClose = Double.parseDouble(row[5]);
            Double volume = Double.parseDouble(row[6]);
            dates.add(date);
            opens.add(open);
            highs.add(high);
            lows.add(low);
            closes.add(close);
            adjCloses.add(adjClose);
            volumes.add(volume);
        }
        List<Double> returns = new ArrayList<>();
        for (int i = 1; i < adjCloses.size(); i++) {
            Double prevClose = adjCloses.get(i - 1);
            Double close = adjCloses.get(i);
            Double ret = Math.log(close / prevClose);
            returns.add(ret);
        }
        List<Double> targets = new ArrayList<>();
        for (int i = 0; i < returns.size() - 1; i++) {
            Double ret = returns.get(i);
            Double target = ret > 0 ? 1.0 : 0.0;
            targets.add(target);
        }
        List<Double> newsSentiments = new ArrayList<>();
        for (int i = 0; i < dates.size(); i++) {
            LocalDate date = dates.get(i);
            Double sentiment = getNewsSentiment(ticker, date);
            newsSentiments.add(sentiment);
        }
        List<Double> socialSentiments = new ArrayList<>();
        for (int i = 0; i < dates.size(); i++) {
            LocalDate date = dates.get(i);
            Double sentiment = getSocialSentiment(ticker, date);
            socialSentiments.add(sentiment);
        }
        List<Double> volatilities = new ArrayList<>();
        for (int
