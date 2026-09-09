# Execution Plan: Add Custom Product Attributes to Product API

## Outcome

- Users and value: Headless storefront applications (React, Angular, etc.) need access to all available custom product attributes through the product detail REST API to display comprehensive product information
- Acceptance evidence: Product detail endpoint returns all custom attributes with their values, types, and display names; API consumers can access custom attributes without additional calls
- Non-goals: Creating new custom attributes, modifying existing attribute definitions, changing attribute business logic
- Approved removals/breaking changes: None

## Evidence

- Selected guide/checklists: Custom REST API, Architecture, Security Checklist, Quality Checklist, Upgrade Compatibility
- Installed versions: Intershop 7.10.41.5-LTS (from build.gradle), REST framework via app_sf_rest cartridges
- Existing behavior and project precedent: 
  - ProductResource uses ProductHandlerImpl from app_sf_rest
  - Custom attributes handled through ProductBOCustomAttributesBOChangesProviderImpl (from ProductBOChangesExtension.extension)
  - Custom attribute types include: Integer (1), Double (2), String (3), Long (8), Boolean (9), Date (10), Decimal (11), Money (12), Quantity (13), Text (14), and collection types (4,5,6,15,16,17,18)
  - Attribute groups and descriptors defined via bc_foundation AttributeDescriptor and AttributeGroup
  - Custom attributes displayed via CustomAttribute.isml template in storefront
- OOTB artifacts inspected (read-only):
  - app_sf_rest/components/contracts.component: ProductResource, ProductHandler contracts
  - app_sf_rest/components/implementations.component: ProductResource, ProductHandlerImpl implementations
  - bc_product/extensions/ProductBOChangesExtension.extension: Custom attribute providers
  - bc_foundation/edl/AttributeDescriptor.edl: Attribute descriptor structure
  - bc_product/templates/inc/ProductAttributeGroup.isml: Attribute group structure
- Supported extension points:
  - ProductHandler contract for business logic extension
  - ProductResource for REST resource extension
  - Mapper extension points for response mapping
  - Component instances and implementations registration

## Decisions

| Decision or assumption | Evidence | Human decision needed? |
|---|---|---|
| Extend existing ProductResource with custom attributes sub-resource | OOTB ProductResource has subResource extension point | No - follows implementation guide recommendation |
| Use existing custom attribute infrastructure (ProductBOCustomAttributesBOChangesProviderImpl) | Custom attributes already accessible via ProductBO | No - reuse existing proven implementation |
| Create project-owned cartridge for REST extension | No existing project cartridge for REST extensions | Yes - cartridge naming and structure |
| Expose all custom attribute types defined in system | CustomAttribute.isml shows 18+ attribute types | No - comprehensive coverage requested |
| Use mapper extension for attribute value conversion | REST framework uses mapper pattern for DTO conversion | No - follows established pattern |
| No authentication changes required | Existing product API already has authentication | No - preserve existing security model |

## Design

- Flow and responsibilities:
  - REST call → ProductResource → ProductHandler → ProductBO → CustomAttributeBOChangesProvider → Attribute value extraction → Mapper → Response
  - Custom attributes extracted from ProductBO via existing BOChangesProvider infrastructure
  - Mapper converts attribute values to REST DTO format with type handling
  - Response includes all available custom attributes with metadata (name, displayName, type, value)

- Contracts/data/configuration:
  - CustomAttributeDTO: ID, name, displayName, type, value (based on attribute type)
  - CustomAttributeListDTO: Collection of custom attributes
  - Extension to ProductResource response to include customAttributes field
  - Component configuration for mapper registration

- Security/privacy:
  - Preserve existing ProductResource ACL and authentication
  - No additional permission requirements
  - Attribute values already filtered by existing business object security

- Persistence/transaction/migration:
  - No database changes required - using existing custom attribute infrastructure
  - No migration needed
  - Read-only access to existing attribute data

- Compatibility and side effects:
  - Backward compatible - adding new field to response
  - No impact on existing product API consumers
  - No performance impact expected (attributes already loaded by ProductBO)

- Failure, rollout, and recovery:
  - If mapper fails, return standard product response without custom attributes
  - Component registration failure logged but doesn't break core product API
  - Rollback by removing cartridge from assembly

- Exact files/modules:
  - New cartridge: app_sf_rest_corporate (or similar project-owned name)
  - CustomAttributeDTO.java in new cartridge
  - CustomAttributeMapper.java in new cartridge  
  - Extended ProductResource or sub-resource in new cartridge
  - Component registration files (contracts.component, implementations.component, instances.component)
  - Module registration for mapper extension

## Steps

1. Create project-owned REST extension cartridge following Custom Cartridge Manual
2. Define CustomAttributeDTO and related data transfer objects
3. Implement CustomAttributeMapper for attribute value conversion
4. Create ProductCustomAttributesResource as sub-resource of ProductResource
5. Register components, implementations, and instances
6. Register mapper extension in appropriate module
7. Add cartridge to relevant assemblies (B2C, B2B as needed)
8. Build and deploy to test environment
9. Test product detail endpoint with custom attributes
10. Verify attribute types and values are correctly returned
11. Verify backward compatibility with existing API consumers

## Verification

- Unit:
  - CustomAttributeMapper tests for all attribute types (1-18)
  - DTO serialization/deserialization tests
  - Mock ProductBO with custom attributes for mapper testing

- Integration/contract:
  - GET /products/{id} returns customAttributes field
  - All attribute types correctly represented in response
  - Localized display names included
  - Empty attribute list for products without custom attributes

- Regression/journey:
  - Existing product detail API calls still work
  - Performance impact negligible (attribute data already loaded)
  - Existing storefront pages unaffected

- Failure/security/non-functional:
  - Invalid/malformed attribute data handled gracefully
  - Authentication/authorization still enforced
  - No exposure of internal system data
  - Response size within acceptable limits

- Commands and expected evidence:
  - Build: `gradlew build` - success
  - Deploy: Cartridge appears in server/cartridges directory
  - API test: `curl -X GET http://server/products/{id}` - includes customAttributes
  - Component verification: Component registry shows new implementations

## Done

- [ ] Acceptance evidence and selected guide pass.
- [ ] Security, quality, and upgrade checklists pass.
- [ ] No OOTB artifact changed or shadowed.
- [ ] No unapproved behavior was removed.
- [ ] Build/tests pass; deployment and recovery are documented.
