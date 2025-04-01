# SeminarDesk Private API Specification

This repository contains the OpenAPI 3.0.3 specification for SeminarDesk's Private API. This specification serves as the source of truth for all Private API documentation and client implementations.

## Overview

The SeminarDesk Private API offers extended functionality for:
- Full event management
- Profile management
- Comprehensive booking and guest management
- Invoices and payments
- Certificates

This API requires special authorization and is intended for:
- Custom administrative interfaces
- Advanced reporting tools
- Deep integrations with business systems
- Internal SeminarDesk use

## Getting Started

### Authentication

The Private API uses API key authentication. Each request must include the `X-API-Key` header with a valid API key.

### Base URL

```
https://{tenantId}.seminardesk.de/private-api
```

Replace `{tenantId}` with your unique tenant identifier assigned by SeminarDesk.

### Documentation

The complete documentation is available at:
- https://app.swaggerhub.com/apis/SeminarDesk/private/v1

## Access Requirements

Access to the Private API:
- Requires a signed agreement with SeminarDesk
- Is subject to rate limits and usage policies
- May incur additional charges depending on your subscription plan

## Support

For questions about the API specification:
- Create an issue in this repository
- Contact support@seminardesk.de
