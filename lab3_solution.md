## Configure AI Applications to Optimize Search Results: Challenge Lab

- **Task 1: Create `metadata.json` file using Cloud Shell Editor**  
   ```shell
   cat <<EOF > metadata.json
   {"id": "doc-1","structData": {"title": "Heaven Resort","category": "information","rating": 4.8},"content": {"mimeType": "application/pdf","uri": "gs://qwiklabs-gcp-04-5788f0cee4c4/hotel1.pdf"}}
   {"id": "doc-2","structData": {"title": "Paradise Reef Resort","category": "information","rating": 4.7},"content": {"mimeType": "application/pdf","uri": "gs://qwiklabs-gcp-04-5788f0cee4c4/hotel2.pdf"}}
   {"id": "doc-3","structData": {"title": "AquaPulse Maldives","category": "information","rating": 4.0},"content": {"mimeType": "application/pdf","uri": "gs://qwiklabs-gcp-04-5788f0cee4c4/hotel3.pdf"}}
   {"id": "doc-4","structData": {"title": "Heaven Resort Financials","category": "financials","rating": 4.8},"content": {"mimeType": "application/pdf","uri": "gs://qwiklabs-gcp-04-5788f0cee4c4/hotel1-financials.pdf"}}
   {"id": "doc-5","structData": {"title": "Paradise Reef Resort Financials","category": "financials","rating": 4.7},"content": {"mimeType": "application/pdf","uri": "gs://qwiklabs-gcp-04-5788f0cee4c4/hotel2-financials.pdf"}}
   {"id": "doc-6","structData": {"title": "AquaPulse Maldives Financials","category": "financials","rating": 4.0},"content": {"mimeType": "application/pdf","uri": "gs://qwiklabs-gcp-04-5788f0cee4c4/hotel3-financials.pdf"}}
   EOF
   ```

- **Upload `metadata.json` file to the Cloud Storage bucket**
   ``` 
   gcloud storage cp ./metadata.json gs://$(gcloud config get-value project)
   ```
  :warning: Sometimes progress assessment fails, please prepare the metadata.json file manually instead of copying and pasting.

- **Task 4: Filtering API**
   - **Define APP_ID**
      ```
      APP_ID=
      ``` 
   - **Category = information**
      ```shell
      curl -X POST -H "Authorization: Bearer $(gcloud auth print-access-token)" \
      -H "Content-Type: application/json" \
      "https://discoveryengine.googleapis.com/v1alpha/projects/$(gcloud projects describe $(gcloud config get-value project) --format="value(projectNumber)")/locations/global/collections/default_collection/engines/$APP_ID/servingConfigs/default_search:search" \
      -d '{
          "query":"What is the revenue for the hotels in the Maldives?",
          "pageSize":10,
          "queryExpansionSpec":{"condition":"AUTO"},
          "spellCorrectionSpec":{"mode":"AUTO"},
          "languageCode":"en-US",
          "facetSpecs":[{"facetKey":{"key":"category"},"limit":30}],
          "userInfo":{"timeZone":"Asia/Bangkok"},
          "filter":"category: ANY(\"information\")"
      }'
      ```
   - **Category = financials**
      ```shell
      curl -X POST -H "Authorization: Bearer $(gcloud auth print-access-token)" \
      -H "Content-Type: application/json" \
      "https://discoveryengine.googleapis.com/v1alpha/projects/$(gcloud projects describe $(gcloud config get-value project) --format="value(projectNumber)")/locations/global/collections/default_collection/engines/$APP_ID/servingConfigs/default_search:search" \
      -d '{
          "query":"What is the revenue for the hotels in the Maldives?",
          "pageSize":10,
          "queryExpansionSpec":{"condition":"AUTO"},
          "spellCorrectionSpec":{"mode":"AUTO"},
          "languageCode":"en-US",
          "contentSearchSpec":{"extractiveContentSpec":{"maxExtractiveAnswerCount":1}},
          "facetSpecs":[{"facetKey":{"key":"category"},"limit":30}],
          "userInfo":{"timeZone":"Asia/Bangkok"},
          "filter":"category: ANY(\"financials\")"
      }'
      ```
- **Task 5: Boost results with higher ratings**
   - **Navigate to Configurations > Control > Filter**
     ```
     rating > 4.1
     ```
   - **Category = financials AND page_size = 2**
      ```shell
      curl -X POST -H "Authorization: Bearer $(gcloud auth print-access-token)" \
      -H "Content-Type: application/json" \
      "https://discoveryengine.googleapis.com/v1alpha/projects/$(gcloud projects describe $(gcloud config get-value project) --format="value(projectNumber)")/locations/global/collections/default_collection/engines/$APP_ID/servingConfigs/default_search:search" \
      -d '{
          "query":"What is the revenue for the hotels in the Maldives?",
          "pageSize":2,
          "queryExpansionSpec":{"condition":"AUTO"},
          "spellCorrectionSpec":{"mode":"AUTO"},
          "languageCode":"en-US",
          "contentSearchSpec":{"extractiveContentSpec":{"maxExtractiveAnswerCount":1}},
          "facetSpecs":[{"facetKey":{"key":"category"},"limit":30}],
          "userInfo":{"timeZone":"Asia/Bangkok"},
          "filter":"category: ANY(\"financials\")"
      }'
      ```
