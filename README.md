import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;
import java.util.*;

public class DplConfigServiceTest {

    @Test
    void testGetCustomOptionsEntry() {
        DplConfigService dplConfigService = DplConfigService.getInstance();
        
        // Mocking DatasetFlowOptionsModel
        DatasetFlowOptionsModel df = Mockito.mock(DatasetFlowOptionsModel.class);
        InputEntityDetailsModel inputEntityDetails = Mockito.mock(InputEntityDetailsModel.class);
        DatasetFlowOptions datasetFlowOptions = Mockito.mock(DatasetFlowOptions.class);
        FileDatasetFlowOption fileOptions = Mockito.mock(FileDatasetFlowOption.class);
        HadoopDatasetFlowOption hadoopOptions = Mockito.mock(HadoopDatasetFlowOption.class);

        // Setting up mock behaviors
        Map<String, String> customOptions = new HashMap<>();
        customOptions.put("glueDatabaseName", "sample_database");
        customOptions.put("glueTableName", "sample_table");

        Mockito.when(fileOptions.getCustomOptions()).thenReturn(customOptions);
        Mockito.when(hadoopOptions.getCustomOptions()).thenReturn(customOptions);
        Mockito.when(datasetFlowOptions.getFileOptions()).thenReturn(fileOptions);
        Mockito.when(datasetFlowOptions.getHadoopOptions()).thenReturn(hadoopOptions);
        Mockito.when(inputEntityDetails.getDatasetFlowOptions()).thenReturn(datasetFlowOptions);

        List<InputEntityDetailsModel> inputDataSets = Collections.singletonList(inputEntityDetails);
        Mockito.when(df.getInputDataSets()).thenReturn(inputDataSets);

        // Calling the method under test
        Optional<Map.Entry<String, String>> result = dplConfigService.getCustomOptionsEntry(df);

        // Assertions
        assertTrue(result.isPresent());
        assertEquals("sample_database", result.get().getKey());
        assertEquals("sample_table", result.get().getValue());
    }
}
