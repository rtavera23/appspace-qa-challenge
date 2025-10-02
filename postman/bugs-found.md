# Bug Report

**ID:** BUG-20251002-API-001  
**Short Title:** GET `/tasks/{id}` returns non-404 for non-existing ID  
**Severity (S2) / Priority (P1):** S2 / P1  
**Environment:** `{{baseUrl}}` (Appspace – QA), Postman 11.x, macOS  
**Build/Date:** 2025-10-02

## Description
Requesting a non-existing task ID does **not** yield **404 Not Found**, which contradicts Swagger for `GET /tasks/{id}`.

## Steps to Reproduce
1. `GET {{baseUrl}}/tasks/00000000-0000-0000-0000-000000000000`  
2. Inspect status code and response body.

## Actual Result
Status code is **not 404** (Postman test fails).

## Expected Result
Return **404 Not Found** for non-existing resources per API contract.

## Evidence
- Latest Postman runner screenshot shows **FAIL** for “404 Not Found”.  
- capture added in the same folder
- 404 Not Found | AssertionError: expected response to have status code 404 but got 200


## Scope/Impact
Clients relying on REST semantics will mis-handle missing resources; error handling and caching strategies break.

## Additional Notes
If the endpoint returns **200** with an empty object/array, the **contract** needs correction or the **implementation** needs to return proper **404**.

## Labels
`backend` `api` `contract` `swagger` `rest`
