- **Task 4: Filtering API**
   - **Define APP_ID**
      ```
      APP_ID=
      ``` 
- **Edit your request to set the page size to 3 and add "searchAsYouTypeSpec": {"condition": "ENABLED"}
      ```shell
      curl -X POST -H "Authorization: Bearer $(gcloud auth print-access-token)" \
      -H "Content-Type: application/json" \
      "https://discoveryengine.googleapis.com/v1alpha/projects/$(gcloud projects describe $(gcloud config get-value project) --format="value(projectNumber)")/locations/global/collections/default_collection/engines/$APP_ID/servingConfigs/default_search:search" \
      -d '{
          "query":"miracle",
          "pageSize":3,
          "queryExpansionSpec":{"condition":"AUTO"},
          "searchAsYouTypeSpec":{"condition":"ENABLED"}
          "spellCorrectionSpec":{"mode":"AUTO"},
          "languageCode":"en-US",
          "userInfo":{"timeZone":"Asia/Bangkok"},
      }'
      ```
