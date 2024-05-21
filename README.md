@Mock
    private ResourceLoader resourceLoader;

    @Mock
    private ObjectMapper objectMapper;

    @InjectMocks
    private DplConfigService dplConfigService;

    @Test
    void testGetControlFileForDatafileName() throws IOException {
        // Arrange
        String dataFileName = "test-filename-$BUSINESS_DATE.csv";
        Resource resource = mock(Resource.class);
        ControlFile expectedControlFile = new ControlFile();
        expectedControlFile.setFileName(dataFileName);
        expectedControlFile.setTokenFile("test-filename-$BUSINESS_DATE.tok");

        InputStream inputStream = mock(InputStream.class);

        when(resourceLoader.getResources(anyString())).thenReturn(new Resource[]{resource});
        when(resource.getInputStream()).thenReturn(inputStream);
        when(objectMapper.readValue(eq(inputStream), eq(ControlFile.class))).thenReturn(expectedControlFile);

        // Act
        ControlFile controlFile = dplConfigService.getControlFileForDatafileName(dataFileName);

        // Assert
        assertNotNull(controlFile);
        assertEquals(dataFileName, controlFile.getFileName());
        assertEquals("test-filename-$BUSINESS_DATE.tok", controlFile.getTokenFile());
    }
