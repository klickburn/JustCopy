void testGetCustomOptionsEntry() {
    DplConfigService dplConfigService = DplConfigService.getInstance();
    DatasetFlowOptionsModel df = new DatasetFlowOptionsModel();
    
    // Mocking the necessary parts of df to simulate a realistic scenario
    InputEntityDetailsModel inputEntityDetails = new InputEntityDetailsModel();
    DatasetFlowOptions options = new DatasetFlowOptions();
    FileDatasetFlowOption fileOptions = new FileDatasetFlowOption();
    HadoopDatasetFlowOption hadoopOptions = new HadoopDatasetFlowOption();
    
    // Set up custom options for file and Hadoop
    Map<String, String> customOptions = new HashMap<>();
    customOptions.put("glueDatabaseName", "sample_database");
    customOptions.put("glueTableName", "sample_table");

    fileOptions.setCustomOptions(customOptions);
    hadoopOptions.setCustomOptions(customOptions);
    
    options.setFileOptions(fileOptions);
    options.setHadoopOptions(hadoopOptions);
    
    inputEntityDetails.setDatasetFlowOptions(options);
    
    List<InputEntityDetailsModel> inputDataSets = new ArrayList<>();
    inputDataSets.add(inputEntityDetails);
    
    df.setInputDataSets(inputDataSets);
    
    Optional<Map.Entry<String, String>> result = dplConfigService.getCustomOptionsEntry(df);
    
    assertTrue(result.isPresent());
    assertEquals("sample_database", result.get().getKey());
    assertEquals("sample_table", result.get().getValue());
}
