# SeminarDesk Private API Specification - Development Guide

## OpenAPI Schema Patterns and Conventions

This document outlines the patterns and conventions used in the SeminarDesk Private API OpenAPI specification.

## File Structure

- **Single file approach**: The API specification is maintained in `private-api.json` (280KB, ~9,800 lines)
- **API docs hub dependency**: External documentation system directly accesses `private-api.json`
- **Modification workflow**: Changes made directly to the single file to maintain compatibility

## Version Management

- **Two API versions**: The specification includes both stable and development versions
  - **v1 (Stable)**: Corresponds to `main` branch - production-ready API
  - **v1-next (Development)**: Corresponds to `next` branch - upcoming version with new features
- **Current branch**: This specification represents the `next` branch (v1-next)

## Naming Conventions

### Parameter Names
- **Case style**: camelCase (e.g., `searchTerm`, `paymentReferenceId`)
- **Pattern**: Descriptive, specific names that clearly indicate purpose
- **Examples**: `searchTerm`, `paymentReferenceId`, `fieldId`

### Schema Names
- **Entity schemas**: PascalCase singular (e.g., `Payment`, `Booking`, `Event`)
- **List schemas**: PascalCase with "List" suffix (e.g., `PaymentsList`, `BookingsList`)  
- **Property schemas**: PascalCase with "Properties" suffix (e.g., `PaymentProperties`)
- **Creation schemas**: PascalCase with "Create" prefix (e.g., `CreatePayment`)

## Endpoint Patterns

### Path Structure
- **Resource collections**: `/{resource}` (e.g., `/payments`, `/bookings`)
- **Resource instances**: `/{resource}/{id}` (e.g., `/payments/{id}`)
- **Nested resources**: `/{parent}/{id}/{child}` (e.g., `/invoices/{id}/payments`)
- **Actions**: `/{resource}/{id}/{action}` (e.g., `/bookings/{id}/cancel`)

### HTTP Methods
- **GET**: Retrieve resource(s), list endpoints support pagination
- **POST**: Create new resources or perform actions
- **Standard operations**: Following RESTful conventions

### Query Parameters
- **Pagination**: `page` parameter using `deepObject` style with `explode: false`
- **Search**: `searchTerm` parameter for text-based filtering
- **Filtering**: Resource-specific filter parameters (e.g., `paymentReferenceId`)
- **Style patterns**:
  - `deepObject` for complex objects (pagination, filters)
  - `form` with `explode: true` for simple parameters

## Schema Patterns

### List Response Structure
```json
{
  "allOf": [
    { "$ref": "#/components/schemas/BaseList" },
    {
      "type": "object",
      "properties": {
        "data": {
          "type": "array",
          "items": { "$ref": "#/components/schemas/EntityName" }
        }
      }
    }
  ]
}
```

### Entity Properties
- **ID fields**: Use `NumericPrimaryKey` reference, marked as `readOnly: true`
- **UUID fields**: Type `string` with `format: uuid`
- **Nullable fields**: Explicitly marked with `nullable: true/false`
- **Descriptions**: Added to complex or business-specific fields

### Required vs Optional
- **Schema composition**: Use `allOf` with separate Properties and RequiredProperties schemas
- **Clear separation**: Properties schema contains field definitions, RequiredProperties defines constraints

## Response Patterns

### Standard HTTP Status Codes
- **200**: Success for GET requests
- **201**: Success for POST requests (creation)
- **401**: API key missing/invalid (includes WWW_Authenticate header)
- **403**: API key lacks permissions
- **501**: Service unavailable or module disabled

### Response Headers
- **401 responses**: Include `WWW_Authenticate` header with `simple` style
- **Content type**: Always `application/json`

### Error Responses
- **Consistent structure**: Use `Error` schema reference
- **Descriptive messages**: Clear indication of what went wrong

## Tags and Organization

### Endpoint Tagging
- **Resource-based**: Tags match primary resource (e.g., "Payments", "Bookings")
- **Multiple tags**: Some endpoints tagged with multiple related resources
- **Consistency**: Plural nouns for resource collections

### Operation IDs
- **Pattern**: `{verb}{ResourceName}` (e.g., `getPayments`, `createBooking`)
- **Descriptive**: Clear indication of operation purpose

## Field Types and Formats

### Common Field Types
- **Dates**: `string` with `format: date`
- **DateTimes**: `string` with `format: date-time` 
- **Numbers**: `number` with `format: double`
- **IDs**: `integer` with `format: int64`, minimum value 1
- **UUIDs**: `string` with `format: uuid`

### Examples and Documentation
- **UUID example**: `"550e8400-e29b-41d4-a716-446655440000"`
- **Amount example**: `23.42` for monetary values
- **Descriptions**: Business context provided for domain-specific fields

## Development Recommendations

### Adding New Endpoints
1. Follow alphabetical order in paths section
2. Use consistent parameter and response patterns
3. Include all standard HTTP status codes
4. Add appropriate tags and operation IDs
5. Create corresponding List schemas for collection endpoints

### Schema Modifications
1. Add new fields to Properties schemas, not directly to entities
2. Maintain nullable field specifications
3. Include descriptions for business-specific fields
4. Follow existing naming conventions

### Large File Management
- **Challenge**: File size causes "conversation too long" errors with AI tools
- **Solution options**: 
  - Use unbundle_openapi_mcp for development workflow
  - Consider split/bundle approach for source vs production
  - Extract specific endpoints for focused work sessions