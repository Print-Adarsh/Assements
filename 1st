# Assements

Ques---Load parse both JSON files and merge the data based on the matching id. 

import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.ObjectMapper;
import java.io.File;
import java.io.IOException;
import java.util.*;
import java.util.stream.Collectors;

class Location {
    public String id;
    public double latitude;
    public double longitude;
}

class Metadata {
    public String id;
    public String type;
    public double rating;
    public int reviews;
}

public class MapDataProcessor {
    public static void main(String[] args) throws IOException {
        ObjectMapper objectMapper = new ObjectMapper();
        List<Location> locations = objectMapper.readValue(new File("locations.json"), new TypeReference<>() {});
        List<Metadata> metadata = objectMapper.readValue(new File("metadata.json"), new TypeReference<>() {});
        
        // Merge data
        Map<String, Location> locationMap = locations.stream()
                .collect(Collectors.toMap(loc -> loc.id, loc -> loc));
        
        List<Map<String, Object>> mergedData = new ArrayList<>();
        for (Metadata meta : metadata) {
            if (locationMap.containsKey(meta.id)) {
                Map<String, Object> merged = new HashMap<>();
                merged.put("id", meta.id);
                merged.put("latitude", locationMap.get(meta.id).latitude);
                merged.put("longitude", locationMap.get(meta.id).longitude);
                merged.put("type", meta.type);
                merged.put("rating", meta.rating);
                merged.put("reviews", meta.reviews);
                mergedData.add(merged);
            }
        }

        // Count valid points per type
        Map<String, Long> countByType = metadata.stream()
                .collect(Collectors.groupingBy(m -> m.type, Collectors.counting()));

        // Calculate average rating per type
        Map<String, Double> avgRatingByType = metadata.stream()
                .collect(Collectors.groupingBy(m -> m.type, Collectors.averagingDouble(m -> m.rating)));

        // Find location with the highest reviews
        Metadata maxReviewLocation = metadata.stream()
                .max(Comparator.comparingInt(m -> m.reviews))
                .orElse(null);

        // Identify locations with incomplete data
        List<String> incompleteDataLocations = new ArrayList<>();
        for (Location loc : locations) {
            boolean hasMetadata = metadata.stream().anyMatch(m -> m.id.equals(loc.id));
            if (!hasMetadata) {
                incompleteDataLocations.add(loc.id);
            }
        }
        
        // Print results
        System.out.println("Valid Points Per Type: " + countByType);
        System.out.println("Average Rating Per Type: " + avgRatingByType);
        if (maxReviewLocation != null) {
            System.out.println("Location with Highest Reviews: " + maxReviewLocation.id + " (" + maxReviewLocation.reviews + " reviews)");
        }
        System.out.println("Locations with Incomplete Data: " + incompleteDataLocations);
    }
}


1.Read JSON Files: Load and parse both JSON files.

2.Merge Data: Combine location data with metadata based on id.

3.Process Data:

Count valid locations per type.

Calculate the average rating per type.

Find the location with the highest number of reviews.

Identify locations with incomplete data (Bonus).

4.Write the output in a structured format.

5.Ensure uniqueness: Instead of standard parsing, we'll use Java Streams for better efficiency and a clear, modular approach.
