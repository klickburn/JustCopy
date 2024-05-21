@Test
void testGetControlFileForDatafileName() throws IOException {
    DplConfigService dplConfigService = DplConfigService.getInstance();
    String dataFileName = "sample-test-filename-$BUSINESS_DATE.csv";

    ControlFile controlFile = dplConfigService.getControlFileForDatafileName(dataFileName);

    assertNotNull(controlFile);
    assertEquals("sample-test-filename-$BUSINESS_DATE.csv", controlFile.getFileName());
    assertEquals("sample-test-filename-$BUSINESS_DATE.tok", controlFile.getTokenFile());
    // Add additional assertions as needed based on the ControlFile class
}
