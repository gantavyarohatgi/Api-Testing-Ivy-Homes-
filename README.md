# Api-Testing-Ivy-Homes-

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

### Sample Request:
```bash
curl "http://35.200.185.69:8000/v1/autocomplete?query=aa&max_results=50"
