# Code-API
package Practicecode;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import org.json.JSONArray;
import org.json.JSONObject;
import java.util.Scanner;

public class WeatherApp5 {
	private static final String API_BASE_URL = "https://samples.openweathermap.org/data/2.5/forecast/hourly";
    private static final String API_KEY = "b6907d289e10d714a6e88b30761fae22";

	public static void main(String[] args) {
		// TODO Auto-generated method stub
	
		        Scanner scanner = new Scanner(System.in);

		        int choice;
		        do {
		            System.out.println("1. Get weather");
		            System.out.println("2. Get Wind Speed");
		            System.out.println("3. Get Pressure");
		            System.out.println("0. Exit");
		            System.out.print("Enter your choice: ");
		            choice = scanner.nextInt();
		            scanner.nextLine(); // Consume the newline character left in the input buffer

		            switch (choice) {
		                case 1:
		                    getWeatherByDate();
		                    break;
		                case 2:
		                    getWindSpeedByDate();
		                    break;
		                case 3:
		                    getPressureByDate();
		                    break;
		                case 0:
		                    System.out.println("Exiting the program.");
		                    break;
		                default:
		                    System.out.println("Invalid choice. Please try again.");
		                    break;
		            }
		        } while (choice != 0);

		        scanner.close();
		    }

		    private static void getWeatherByDate() {
		        Scanner scanner = new Scanner(System.in);
		        System.out.print("Enter the date (YYYY-MM-DD HH:mm:ss): ");
		        String date = scanner.nextLine();

		        String apiUrl = API_BASE_URL + "?q=London,us&appid=" + API_KEY;
		        try {
		            String response = makeApiCall(apiUrl);
		            if (response != null) {
		                JSONObject jsonObject = new JSONObject(response);
		                JSONArray weatherData = jsonObject.getJSONArray("list");
		                for (int i = 0; i < weatherData.length(); i++) {
		                    JSONObject forecast = weatherData.getJSONObject(i);
		                    String forecastDateTime = forecast.getString("dt_txt");
		                    if (forecastDateTime.equals(date)) {
		                        double temperature = forecast.getJSONObject("main").getDouble("temp");
		                        System.out.println("Temperature on " + date + ": " + temperature + "°C");
		                        return;
		                    }
		                }
		                System.out.println("No weather data available for the specified date.");
		            } else {
		                System.out.println("Failed to fetch weather data.");
		            }
		        } catch (Exception e) {
		            System.out.println("Error occurred while processing the data: " + e.getMessage());
		        }
		    }

		    private static void getWindSpeedByDate() {
		        Scanner scanner = new Scanner(System.in);
		        System.out.print("Enter the date (YYYY-MM-DD HH:mm:ss): ");
		        String date = scanner.nextLine();

		        String apiUrl = API_BASE_URL + "?q=London,us&appid=" + API_KEY;
		        try {
		            String response = makeApiCall(apiUrl);
		            if (response != null) {
		                JSONObject jsonObject = new JSONObject(response);
		                JSONArray weatherData = jsonObject.getJSONArray("list");
		                for (int i = 0; i < weatherData.length(); i++) {
		                    JSONObject forecast = weatherData.getJSONObject(i);
		                    String forecastDateTime = forecast.getString("dt_txt");
		                    if (forecastDateTime.equals(date)) {
		                        double windSpeed = forecast.getJSONObject("wind").getDouble("speed");
		                        System.out.println("Wind Speed on " + date + ": " + windSpeed + " m/s");
		                        return;
		                    }
		                }
		                System.out.println("No wind speed data available for the specified date.");
		            } else {
		                System.out.println("Failed to fetch weather data.");
		            }
		        } catch (Exception e) {
		            System.out.println("Error occurred while processing the data: " + e.getMessage());
		        }
		    }

		    private static void getPressureByDate() {
		        Scanner scanner = new Scanner(System.in);
		        System.out.print("Enter the date (YYYY-MM-DD HH:mm:ss): ");
		        String date = scanner.nextLine();

		        String apiUrl = API_BASE_URL + "?q=London,us&appid=" + API_KEY;
		        try {
		            String response = makeApiCall(apiUrl);
		            if (response != null) {
		                JSONObject jsonObject = new JSONObject(response);
		                JSONArray weatherData = jsonObject.getJSONArray("list");
		                for (int i = 0; i < weatherData.length(); i++) {
		                    JSONObject forecast = weatherData.getJSONObject(i);
		                    String forecastDateTime = forecast.getString("dt_txt");
		                    if (forecastDateTime.equals(date)) {
		                        double pressure = forecast.getJSONObject("main").getDouble("pressure");
		                        System.out.println("Pressure on " + date + ": " + pressure + " hPa");
		                        return;
		                    }
		                }
		                System.out.println("No pressure data available for the specified date.");
		            } else {
		                System.out.println("Failed to fetch weather data.");
		            }
		        } catch (Exception e) {
		            System.out.println("Error occurred while processing the data: " + e.getMessage());
		        }
		    }

		    private static String makeApiCall(String apiUrl) throws IOException {
		        StringBuilder responseBuilder = new StringBuilder();
		        URL url = new URL(apiUrl);
		        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
		        connection.setRequestMethod("GET");
		        int responseCode = connection.getResponseCode();

		        if (responseCode == HttpURLConnection.HTTP_OK) {
		            try (BufferedReader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()))) {
		                String line;
		                while ((line = reader.readLine()) != null) {
		                    responseBuilder.append(line);
		                }
		            }
		            return responseBuilder.toString();
		        } else {
		            System.out.println("API request failed with response code: " + responseCode);
		            return null;
		        }
		    }
		}
