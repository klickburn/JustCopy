import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.Test;

import java.util.*;

class DplConfigServiceTest {

    private DplConfigService dplConfigService = new DplConfigService();

    @Test
    void testGetCustomOptionsEntry_ValidData() {
        // Arrange
        DatasetFlowOptionsModel df = new DatasetFlowOptionsModel();
        InputEntityDetailsModel ied = new InputEntityDetailsModel();
        DatasetFlowOptions datasetFlowOptions = new DatasetFlowOptions();
        FileDatasetFlowOption fileOptions = new FileDatasetFlowOption();

        Map<String, String> customOptions = new HashMap<>();
        customOptions.put("glueDatabaseName", "sampleDatabase");
        customOptions.put("glueTableName", "sampleTable");

        fileOptions.setCustomOptions(customOptions);
        datasetFlowOptions.setFileOptions(fileOptions);
        ied.setDatasetFlowOptions(datasetFlowOptions);
        df.setInputDataSets(Collections.singletonList(ied));

        // Act
        Optional<Map.Entry<String, String>> result = dplConfigService.getCustomOptionsEntry(df);

        // Assert
        assertTrue(result.isPresent(), "Custom options entry should be present");
        assertEquals("sampleDatabase", result.get().getKey(), "Database name should match");
        assertEquals("sampleTable", result.get().getValue(), "Table name should match");
    }

    @Test
    void testGetCustomOptionsEntry_NullInput() {
        // Act
        Optional<Map.Entry<String, String>> result = dplConfigService.getCustomOptionsEntry(null);

        // Assert
        assertFalse(result.isPresent(), "Result should be empty when input is null");
    }

    @Test
    void testGetCustomOptionsEntry_EmptyInputDataSets() {
        // Arrange
        DatasetFlowOptionsModel df = new DatasetFlowOptionsModel();
        df.setInputDataSets(Collections.emptyList());

        // Act
        Optional<Map.Entry<String, String>> result = dplConfigService.getCustomOptionsEntry(df);

        // Assert
        assertFalse(result.isPresent(), "Result should be empty when input data sets are empty");
    }

    @Test
    void testGetCustomOptionsEntry_NullDatasetFlowOptions() {
        // Arrange
        DatasetFlowOptionsModel df = new DatasetFlowOptionsModel();
        InputEntityDetailsModel ied = new InputEntityDetailsModel();
        ied.setDatasetFlowOptions(null);
        df.setInputDataSets(Collections.singletonList(ied));

        // Act
        Optional<Map.Entry<String, String>> result = dplConfigService.getCustomOptionsEntry(df);

        // Assert
        assertFalse(result.isPresent(), "Result should be empty when dataset flow options are null");
    }

    @Test
    void testGetCustomOptionsEntry_NullFileAndHadoopOptions() {
        // Arrange
        DatasetFlowOptionsModel df = new DatasetFlowOptionsModel();
        InputEntityDetailsModel ied = new InputEntityDetailsModel();
        DatasetFlowOptions datasetFlowOptions = new DatasetFlowOptions();
        datasetFlowOptions.setFileOptions(null);
        datasetFlowOptions.setHadoopOptions(null);
        ied.setDatasetFlowOptions(datasetFlowOptions);
        df.setInputDataSets(Collections.singletonList(ied));

        // Act
        Optional<Map.Entry<String, String>> result = dplConfigService.getCustomOptionsEntry(df);

        // Assert
        assertFalse(result.isPresent(), "Result should be empty when file and Hadoop options are null");
    }
}
