Certainly! Below is the full `DplConfigServiceTest` class, including the complete `testGetCustomOptionsEntry` and `testGetCustomOptionsEntry_Empty` methods.

```java
import org.junit.jupiter.api.Test;
import java.util.Optional;
import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;
import java.io.IOException;
import java.util.List;
import java.util.Map;
import org.junit.jupiter.api.BeforeEach;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;
import org.springframework.core.io.Resource;
import org.springframework.core.io.support.ResourcePatternResolver;

public class DplConfigServiceTest {

    @Mock
    private ResourcePatternResolver resourceResolver;

    @InjectMocks
    private DplConfigService dplConfigService;

    @Mock
    private Resource resource;

    @BeforeEach
    void setUp() {
        MockitoAnnotations.openMocks(this);
    }

    @Test
    void testGetByDatasetByFilename() {
        DplConfigService dplConfigService = DplConfigService.getInstance();
        Optional<DatasetConfig> optional = dplConfigService.getDatasetConfigByDataFileName("sample-test-filename");
        DatasetConfig pipelineConfig = optional.get();
        assertNotNull(pipelineConfig);
        assertEquals("SAMPLE_TEST", pipelineConfig.getDatasetName());
        assertEquals("sample-test-filename-$BUSINESS_DATE.csv", pipelineConfig.getDataFileName());
        assertEquals("sample-test-filename-$BUSINESS_DATE.tok", pipelineConfig.getTokenFileName());
        assertEquals("c3e0c181-6d51-4396-a3cf-c11bb7ccafdc", pipelineConfig.getPipelineGuid());
        assertEquals("303c9ddf-3204-3edf-a712-63e719fdeca2", pipelineConfig.getDatasetGuid());
        assertEquals("1.0", pipelineConfig.getDatasetVersion());
        assertEquals("30673", pipelineConfig.getSourceSealId());
        assertFalse(pipelineConfig.isPci());
        assertFalse(pipelineConfig.isMultiDataFile());
    }

    @Test
    void testGetByDatasetGuid() {
        DplConfigService dplConfigService = DplConfigService.getInstance();
        Optional<DatasetConfig> optional = dplConfigService.getDatasetConfigByDatasetGuid("303c9ddf-3204-3edf-a712-63e719fdeca2");
        DatasetConfig pipelineConfig = optional.get();
        assertNotNull(pipelineConfig);
        assertEquals("SAMPLE_TEST", pipelineConfig.getDatasetName());
        assertEquals("sample-test-filename-$BUSINESS_DATE.csv", pipelineConfig.getDataFileName());
        assertEquals("sample-test-filename-$BUSINESS_DATE.tok", pipelineConfig.getTokenFileName());
        assertEquals("c3e0c181-6d51-4396-a3cf-c11bb7ccafdc", pipelineConfig.getPipelineGuid());
        assertEquals("303c9ddf-3204-3edf-a712-63e719fdeca2", pipelineConfig.getDatasetGuid());
        assertEquals("1.0", pipelineConfig.getDatasetVersion());
        assertEquals("30673", pipelineConfig.getSourceSealId());
        assertFalse(pipelineConfig.isPci());
        assertFalse(pipelineConfig.isMultiDataFile());
    }

    @Test
    void testGetByDatasetName() {
        DplConfigService dplConfigService = DplConfigService.getInstance();
        Optional<DatasetConfig> optional = dplConfigService.getDatasetConfigByDatasetName("SAMPLE_TEST");
        DatasetConfig pipelineConfig = optional.get();
        assertNotNull(pipelineConfig);
        assertEquals("SAMPLE_TEST", pipelineConfig.getDatasetName());
        assertEquals("sample-test-filename-$BUSINESS_DATE.csv", pipelineConfig.getDataFileName());
        assertEquals("sample-test-filename-$BUSINESS_DATE.tok", pipelineConfig.getTokenFileName());
        assertEquals("c3e0c181-6d51-4396-a3cf-c11bb7ccafdc", pipelineConfig.getPipelineGuid());
        assertEquals("303c9ddf-3204-3edf-a712-63e719fdeca2", pipelineConfig.getDatasetGuid());
        assertEquals("1.0", pipelineConfig.getDatasetVersion());
        assertEquals("30673", pipelineConfig.getSourceSealId());
        assertFalse(pipelineConfig.isPci());
        assertFalse(pipelineConfig.isMultiDataFile());
    }

    @Test
    void testMissingFiles() {
        DplConfigService dplConfigService = DplConfigService.getInstance();
        Optional<DatasetConfig> optional = dplConfigService.getDatasetConfigByDatasetName("KNOWN_NON_EXISTENT");
        assertFalse(optional.isPresent());
    }

    // New tests for highlighted lines

    @Test
    void testGetControlFileForDataFileName() throws IOException {
        String dataFileName = "testFile.txt";
        String expectedFileName = "testFile.txt";
        ControlFile controlFile = new ControlFile();
        controlFile.setGetFileName(expectedFileName);

        when(resourceResolver.getResources(anyString())).thenReturn(new Resource[]{resource});
        when(resource.getInputStream()).thenReturn(any());

        ControlFile result = dplConfigService.getControlFileForDataFileName(dataFileName);

        assertNotNull(result);
        assertEquals(expectedFileName, result.getGetFileName());
    }

    @Test
    void testGetControlFileForDataFileName_Null() throws IOException {
        String dataFileName = "testFile.txt";

        when(resourceResolver.getResources(anyString())).thenReturn(new Resource[]{});

        ControlFile result = dplConfigService.getControlFileForDataFileName(dataFileName);

        assertNull(result);
    }

    @Test
    void testGetControlFileForPipelineGuid() throws IOException {
        String pipelineGuid = "testGuid";
        String expectedFileName = "testFile.txt";
        ControlFile controlFile = new ControlFile();
        controlFile.setGetFileName(expectedFileName);

        when(resourceResolver.getResources(anyString())).thenReturn(new Resource[]{resource});
        when(resource.getInputStream()).thenReturn(any());

        ControlFile result = dplConfigService.getControlFileForPipelineGuid(pipelineGuid);

        assertNotNull(result);
        assertEquals(expectedFileName, result.getGetFileName());
    }

    @Test
    void testGetControlFileForPipelineGuid_Null() throws IOException {
        String pipelineGuid = "testGuid";

        when(resourceResolver.getResources(anyString())).thenReturn(new Resource[]{});

        ControlFile result = dplConfigService.getControlFileForPipelineGuid(pipelineGuid);

        assertNull(result);
    }

    @Test
    void testGetGlueDetailByDatasetName() {
        String datasetName = "testDataset";
        String expectedGlueDatabaseName = "testDatabase";
        String expectedGlueTableName = "testTable";

        DatasetFlowOptionsModel datasetFlowOptionsModel = mock(DatasetFlowOptionsModel.class);
        when(dplConfigService.getFlowDefinitionForDatasetName(datasetName)).thenReturn(datasetFlowOptionsModel);
        when(datasetFlowOptionsModel.getCustomOptions()).thenReturn(Map.of(
            "glueDatabaseName", expectedGlueDatabaseName,
            "glueTableName", expectedGlueTableName
        ));

        Optional<Map.Entry<String, String>> result = dplConfigService.getGlueDetailByDatasetName(datasetName);

        assertTrue(result.isPresent());
        assertEquals(expectedGlueDatabaseName, result.get().getKey());
        assertEquals(expectedGlueTableName, result.get().getValue());
    }

    @Test
    void testGetGlueDetailByDatasetName_Empty() {
        String datasetName = "testDataset";

        when(dplConfigService.getFlowDefinitionForDatasetName(datasetName)).thenReturn(null);

        Optional<Map.Entry<String, String>> result = dplConfigService.getGlueDetailByDatasetName(datasetName);

        assertFalse(result.isPresent());
    }

    @Test
    void testGetGlueDetailByPipelineGuid() {
        String pipelineGuid = "testGuid";
        String expectedGlueDatabaseName = "testDatabase";
        String expectedGlueTableName = "testTable";

        DatasetFlowOptionsModel datasetFlowOptionsModel = mock(DatasetFlowOptionsModel.class);
        when(dplConfigService.getFlowDefinitionForPipelineGuid(pipelineGuid)).thenReturn(datasetFlowOptionsModel);
        when(datasetFlowOptionsModel.getCustomOptions()).thenReturn(Map.of(
            "glueDatabaseName", expectedGlueDatabaseName,
            "glueTableName", expectedGlueTableName
        ));

        Optional<Map.Entry<String, String>> result = dplConfigService.getGlueDetailByPipelineGuid(pipelineGuid);

        assertTrue(result.isPresent());
        assertEquals(expectedGlueDatabaseName, result.get().getKey());
        assertEquals(expectedGlueTableName, result.get().getValue());
    }

    @Test
    void testGetGlueDetailByPipelineGuid_Empty() {
        String pipelineGuid = "testGuid";

        when(dplConfigService.getFlowDefinitionForPipelineGuid(pipelineGuid)).thenReturn(null);

        Optional<Map.Entry<String, String>> result = dplConfigService.getGlueDetailByPipelineGuid(pipelineGuid);

        assertFalse(result.isPresent());
    }

    @Test
    void testGetCustomOptionsEntry() {
        DatasetFlowOptionsModel datasetFlowOptionsModel = new DatasetFlowOptionsModel();
        datasetFlowOptionsModel.setInputDataSets(List.of(new InputEntityDetailsModel()));
        InputEntityDetailsModel inputEntityDetailsModel = datasetFlowOptionsModel.getInputDataSets().get(0);
        FileDatasetFlowOptions fileDatasetFlowOptions = new FileDatasetFlowOptions();
        fileDatasetFlowOptions.setCustomOptions(Map.of(
            "glueDatabaseName", "testDatabase",
            "glueTableName
