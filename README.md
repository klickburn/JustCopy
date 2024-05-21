import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;
import java.util.*;

public class DplConfigServiceTest {

    @Test
    void testGetCustomOptionsEntry_withNullDatasetFlowOptionsModel() {
        DplConfigService dplConfigService = DplConfigService.getInstance();
        DatasetFlowOptionsModel df = null;

        Optional<Map.Entry<String, String>> result = dplConfigService.getCustomOptionsEntry(df);

        assertFalse(result.isPresent());
    }

    @Test
    void testGetCustomOptionsEntry_withEmptyInputDataSets() {
        DplConfigService dplConfigService = DplConfigService.getInstance();
        DatasetFlowOptionsModel df = new DatasetFlowOptionsModel();
        df.setInputDataSets(new ArrayList<>());

        Optional<Map.Entry<String, String>> result = dplConfigService.getCustomOptionsEntry(df);

        assertFalse(result.isPresent());
    }

    @Test
    void testGetCustomOptionsEntry_withNullInputEntityDetailsModel() {
        DplConfigService dplConfigService = DplConfigService.getInstance();
        DatasetFlowOptionsModel df = new DatasetFlowOptionsModel();
        df.setInputDataSets(Arrays.asList((InputEntityDetailsModel) null));

        Optional<Map.Entry<String, String>> result = dplConfigService.getCustomOptionsEntry(df);

        assertFalse(result.isPresent());
    }

    @Test
    void testGetCustomOptionsEntry_withNullDatasetFlowOptions() {
        DplConfigService dplConfigService = DplConfigService.getInstance();
        DatasetFlowOptionsModel df = new DatasetFlowOptionsModel();
        InputEntityDetailsModel inputEntityDetails = new InputEntityDetailsModel();
        df.setInputDataSets(Arrays.asList(inputEntityDetails));

        Optional<Map.Entry<String, String>> result = dplConfigService.getCustomOptionsEntry(df);

        assertFalse(result.isPresent());
    }

    @Test
    void testGetCustomOptionsEntry_withNullFileAndHadoopOptions() {
        DplConfigService dplConfigService = DplConfigService.getInstance();
        DatasetFlowOptionsModel df = new DatasetFlowOptionsModel();
        InputEntityDetailsModel inputEntityDetails = new InputEntityDetailsModel();
        DatasetFlowOptions datasetFlowOptions = new DatasetFlowOptions();
        inputEntityDetails.setDatasetFlowOptions(datasetFlowOptions);
        df.setInputDataSets(Arrays.asList(inputEntityDetails));

        Optional<Map.Entry<String, String>> result = dplConfigService.getCustomOptionsEntry(df);

        assertFalse(result.isPresent());
    }

    @Test
    void testGetCustomOptionsEntry_withMissingCustomOptions() {
        DplConfigService dplConfigService = DplConfigService.getInstance();
        DatasetFlowOptionsModel df = new DatasetFlowOptionsModel();
        InputEntityDetailsModel inputEntityDetails = new InputEntityDetailsModel();
        DatasetFlowOptions datasetFlowOptions = new DatasetFlowOptions();
        FileDatasetFlowOption fileOptions = new FileDatasetFlowOption();
        HadoopDatasetFlowOption hadoopOptions = new HadoopDatasetFlowOption();

        datasetFlowOptions.setFileOptions(fileOptions);
        datasetFlowOptions.setHadoopOptions(hadoopOptions);
        inputEntityDetails.setDatasetFlowOptions(datasetFlowOptions);
        df.setInputDataSets(Arrays.asList(inputEntityDetails));

        Optional<Map.Entry<String, String>> result = dplConfigService.getCustomOptionsEntry(df);

        assertFalse(result.isPresent());
    }

    @Test
    void testGetCustomOptionsEntry_withMissingGlueKeys() {
        DplConfigService dplConfigService = DplConfigService.getInstance();
        DatasetFlowOptionsModel df = new DatasetFlowOptionsModel();
        InputEntityDetailsModel inputEntityDetails = new InputEntityDetailsModel();
        DatasetFlowOptions datasetFlowOptions = new DatasetFlowOptions();
        FileDatasetFlowOption fileOptions = new FileDatasetFlowOption();
        HadoopDatasetFlowOption hadoopOptions = new HadoopDatasetFlowOption();

        Map<String, String> customOptions = new HashMap<>();
        fileOptions.setCustomOptions(customOptions);
        hadoopOptions.setCustomOptions(customOptions);

        datasetFlowOptions.setFileOptions(fileOptions);
        datasetFlowOptions.setHadoopOptions(hadoopOptions);
        inputEntityDetails.setDatasetFlowOptions(datasetFlowOptions);
        df.setInputDataSets(Arrays.asList(inputEntityDetails));

        Optional<Map.Entry<String, String>> result = dplConfigService.getCustomOptionsEntry(df);

        assertFalse(result.isPresent());
    }

    @Test
    void testGetCustomOptionsEntry_withValidCustomOptions() {
        DplConfigService dplConfigService = DplConfigService.getInstance();
        DatasetFlowOptionsModel df = new DatasetFlowOptionsModel();
        InputEntityDetailsModel inputEntityDetails = new InputEntityDetailsModel();
        DatasetFlowOptions datasetFlowOptions = new DatasetFlowOptions();
        FileDatasetFlowOption fileOptions = new FileDatasetFlowOption();
        HadoopDatasetFlowOption hadoopOptions = new HadoopDatasetFlowOption();

        Map<String, String> customOptions = new HashMap<>();
        customOptions.put("glueDatabaseName", "sample_database");
        customOptions.put("glueTableName", "sample_table");

        fileOptions.setCustomOptions(customOptions);
        hadoopOptions.setCustomOptions(customOptions);

        datasetFlowOptions.setFileOptions(fileOptions);
        datasetFlowOptions.setHadoopOptions(hadoopOptions);
        inputEntityDetails.setDatasetFlowOptions(datasetFlowOptions);
        df.setInputDataSets(Arrays.asList(inputEntityDetails));

        Optional<Map.Entry<String, String>> result = dplConfigService.getCustomOptionsEntry(df);

        assertTrue(result.isPresent());
        assertEquals("sample_database", result.get().getKey());
        assertEquals("sample_table", result.get().getValue());
    }
}
