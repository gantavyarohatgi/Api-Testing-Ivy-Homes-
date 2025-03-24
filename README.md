# Autocomplete API Documentation

This repository documents the behavior, findings, and best practices for interacting with the **Autocomplete API** hosted at `http://35.200.185.69:8000`.  
The API provides autocomplete suggestions based on input prefixes. It operates across different versions with varying input character types and constraints.

---

## API Versions

1. **Version 1 (v1)**  
   - **Endpoint:** `/v1/autocomplete?query=<string>&max_results=<number>`  
   - **Supports:** Lowercase English alphabets only (`a-z`).  
   
2. **Version 2 (v2)**  
   - **Endpoint:** `/v2/autocomplete?query=<string>&max_results=<number>`  
   - **Supports:** Lowercase English alphabets (`a-z`) and numbers (`0-9`).  

3. **Version 3 (v3)**  
   - **Endpoint:** `/v3/autocomplete?query=<string>&max_results=<number>`  
   - **Supports:** Lowercase English alphabets, numbers, and special characters (e.g., `@`, `#`, `%`, etc.).  

---

## Step-by-Step Testing Process

### 1. Initial API Queries  
   - I started by querying the API directly from my **browser** (using the base endpoint).  
   - After a couple of queries, I switched between different **API versions (v1, v2, and v3)** to explore the differences.

### 2. Using Postman  
   - I then opened **Postman** to test the API further.  
   - The first problem I wanted to solve was to **increase the count of results** returned by the API.  
     - I tried adding various parameters to the query string, and after multiple failed attempts, I discovered that the **`max_results`** parameter worked.  
     - However, even with `max_results=1000`, the API returned a **maximum of 50 results** only.

   - I decided to move on to the next step.

### 3. Checking the Rate Limit  
   - Next, I tested the API’s **rate limit** by writing a Python script on **Google Colab**.  
   - The script continuously queried the API without any delay to observe its response to rapid consecutive requests.  
   - **Finding:** After **50 continuous queries**, I encountered a "Too Many Requests for URL" error.  
   - **Solution:** I implemented a simple fix by adding a **0.2-second delay** between consecutive queries, which solved the rate-limiting issue.

### 4. Extracting All Words from the Dictionary  
   - My next goal was to **extract all possible words** from the autocomplete system.  
   - I wrote a Python script that recursively queries the API with different prefixes.  
   - **Challenge:** Since there are often **more than 50 words** for each prefix (especially for common starting letters like "a"), I had to make multiple queries for each prefix by extending it further.  
   - This approach allowed me to systematically extract words beyond the 50-result limit by diving deeper into the prefix hierarchy.

---

## Tools Used

1. **Web Browser** – Used for initial API queries.  
2. **Postman** – For testing various query parameters and analyzing API behavior.  
3. **Google Colab** – For writing Python scripts to test rate limits, handle pagination, and extract all words from the API.  
4. **Python** – Core programming language used to automate API interactions and handle rate limits, recursive queries, and result extraction.

---

## API Parameters

### Query Parameter:  
- **`query`** – The string prefix to be used for autocomplete suggestions.  

### Optional Parameter:  
- **`max_results`** – Specifies the maximum number of autocomplete suggestions to return. The default number of result is 10.

---

## Findings & Behavior

1. **Default Result Count:**  
   - By default, the API returns **10 results** if no `max_results` parameter is specified.

2. **Maximum Result Count:**  
   - The maximum value for `max_results` is **50**. Any value higher than this will still return only **50 results**.

3. **Rate Limiting:**  
   - The API crashes when **50 continuous queries** are sent without any delay.  
   - **Solution:** To prevent hitting the rate limit, a delay of **0.2 seconds** has been implemented between consecutive API calls.  

---

## Usage Example: Version 1

### Sample Request:
"http://35.200.185.69:8000/v1/autocomplete?query=aa&max_results=50"

### Sample Response:
{
  "version": "v1",
  "count": 10,
  "results": [
    "aa",
    "aabdknlvkc",
    "aabrkcd",
    "aadgdqrwdy",
    "aagqg",
    "aaiha",
    "aainmxg",
    "aajfebume",
    "aajwv",
    "aakfubvxv"
  ]
}
