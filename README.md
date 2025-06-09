# Trust Asset Chaincode - Hyperledger Fabric

A comprehensive asset management smart contract built for Hyperledger Fabric blockchain networks. This chaincode provides secure, transparent, and immutable asset tracking capabilities with full CRUD operations.

## 🚀 Quick Start

### Prerequisites

- Go 1.18 or higher
- Hyperledger Fabric v2.x network
- Git

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/gorillaether/trust-asset-chaincode.git
   cd trust-asset-chaincode-go
   ```

2. **Install dependencies:**
   ```bash
   go mod download
   ```

3. **Run tests:**
   ```bash
   go test ./chaincode/...
   ```

## 📋 Table of Contents

- [Architecture Overview](#architecture-overview)
- [Asset Structure](#asset-structure)
- [Smart Contract Functions](#smart-contract-functions)
- [Deployment Guide](#deployment-guide)
- [API Reference](#api-reference)
- [Testing](#testing)
- [Development Guide](#development-guide)
- [Security Considerations](#security-considerations)
- [Performance Optimization](#performance-optimization)
- [Troubleshooting](#troubleshooting)
- [Possible Enhancements](#possible-enhancements)
- [Contributing](#contributing)
- [License](#license)

## 🏗️ Architecture Overview

This chaincode implements a basic asset management system with the following components:

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Client App    │───▶│  Fabric Network │───▶│   Chaincode     │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                                        │
                                                        ▼
                                               ┌─────────────────┐
                                               │   World State   │
                                               │   (CouchDB/     │
                                               │   LevelDB)      │
                                               └─────────────────┘
```

### Key Features

- ✅ **Asset Creation** - Create new assets with unique identifiers
- ✅ **Asset Reading** - Retrieve asset information by ID
- ✅ **Asset Updates** - Modify existing asset properties
- ✅ **Asset Deletion** - Remove assets from the ledger
- ✅ **Ownership Transfer** - Secure transfer of asset ownership
- ✅ **Asset Listing** - Query all assets in the system
- ✅ **Existence Checking** - Verify asset presence before operations

## 📊 Asset Structure

Each asset in the system contains the following properties:

```go
type Asset struct {
    AppraisedValue int    `json:"AppraisedValue"` // Asset monetary value
    Color          string `json:"Color"`          // Asset color identifier
    ID             string `json:"ID"`             // Unique asset identifier
    Owner          string `json:"Owner"`          // Current owner name
    Size           int    `json:"Size"`           // Asset size metric
}
```

### Field Descriptions

| Field | Type | Description | Constraints |
|-------|------|-------------|-------------|
| `ID` | string | Unique identifier for the asset | Required, immutable |
| `Color` | string | Color classification of the asset | Required |
| `Size` | int | Size measurement of the asset | Required, positive integer |
| `Owner` | string | Current owner of the asset | Required |
| `AppraisedValue` | int | Monetary value of the asset | Required, positive integer |

## 🔧 Smart Contract Functions

### Core Operations

#### `InitLedger()`
Initializes the ledger with sample assets for testing and demonstration.

**Sample Assets Created:**
- asset1: Blue, Size 5, Owner: Tomoko, Value: $300
- asset2: Red, Size 5, Owner: Brad, Value: $400
- asset3: Green, Size 10, Owner: Jin Soo, Value: $500
- asset4: Yellow, Size 10, Owner: Max, Value: $600
- asset5: Black, Size 15, Owner: Adriana, Value: $700
- asset6: White, Size 15, Owner: Michel, Value: $800

#### `CreateAsset(id, color, size, owner, appraisedValue)`
Creates a new asset with the specified parameters.

**Parameters:**
- `id` (string): Unique asset identifier
- `color` (string): Asset color
- `size` (int): Asset size
- `owner` (string): Initial owner
- `appraisedValue` (int): Asset value

**Returns:** Error if asset already exists or creation fails

#### `ReadAsset(id)`
Retrieves an asset by its unique identifier.

**Parameters:**
- `id` (string): Asset identifier

**Returns:** Asset object or error if not found

#### `UpdateAsset(id, color, size, owner, appraisedValue)`
Updates all properties of an existing asset.

**Parameters:** Same as CreateAsset
**Returns:** Error if asset doesn't exist or update fails

#### `DeleteAsset(id)`
Removes an asset from the world state.

**Parameters:**
- `id` (string): Asset identifier

**Returns:** Error if asset doesn't exist or deletion fails

#### `TransferAsset(id, newOwner)`
Transfers ownership of an asset to a new owner.

**Parameters:**
- `id` (string): Asset identifier
- `newOwner` (string): New owner name

**Returns:** Previous owner name or error

#### `GetAllAssets()`
Retrieves all assets from the world state.

**Returns:** Array of all assets or error

#### `AssetExists(id)`
Checks if an asset exists in the world state.

**Parameters:**
- `id` (string): Asset identifier

**Returns:** Boolean indicating existence or error

## 🚀 Deployment Guide

For detailed deployment instructions, see [DEPLOYMENT.md](docs/DEPLOYMENT.md).

### Quick Deployment

```bash
# Package chaincode
peer lifecycle chaincode package trust-asset.tar.gz --path . --lang golang --label trust-asset_1.0

# Install and approve (see full guide for complete steps)
peer lifecycle chaincode install trust-asset.tar.gz
```

## 📚 API Reference

For complete API documentation, see [API.md](docs/API.md).

### Example Usage

```bash
# Create an asset
peer chaincode invoke -C mychannel -n trust-asset \
    -c '{"function":"CreateAsset","Args":["asset7","purple","20","Alice","1000"]}'

# Read an asset
peer chaincode query -C mychannel -n trust-asset \
    -c '{"function":"ReadAsset","Args":["asset7"]}'
```

## 🧪 Testing

```bash
# Run all tests
go test ./chaincode/...

# Run with coverage
go test -cover ./chaincode/...

# Generate coverage report
go test -coverprofile=coverage.out ./chaincode/...
go tool cover -html=coverage.out -o coverage.html
```

## 🚀 Possible Enhancements

### 1. Advanced Asset Features
- **Asset Categories**: Add asset type classification system
- **Metadata Support**: Include flexible custom asset metadata
- **Asset History**: Track complete lifecycle and ownership history
- **Batch Operations**: Support bulk asset creation and updates
- **Asset Relationships**: Link related assets together
- **Versioning**: Track asset modifications over time

### 2. Enhanced Security & Access Control
- **Multi-signature Support**: Require multiple approvals for high-value transfers
- **Role-Based Access Control (RBAC)**: Define roles like admin, operator, viewer
- **Time-locked Transfers**: Implement delayed ownership changes
- **Approval Workflows**: Multi-step approval processes for sensitive operations
- **Audit Trails**: Comprehensive logging of all operations
- **Digital Signatures**: Cryptographic proof of authenticity

### 3. Business Logic Extensions
- **Asset Valuation Engine**: Automatic appraisal updates based on market data
- **Marketplace Features**: Built-in trading and auction capabilities
- **Escrow Services**: Secure transaction intermediary services
- **Fractional Ownership**: Support for shared asset ownership
- **Licensing & Royalties**: Track usage rights and payment distributions
- **Insurance Integration**: Connect with insurance providers

### 4. Integration & Interoperability
- **REST API Gateway**: HTTP interface for web applications
- **GraphQL Support**: Flexible query interface
- **Event Streaming**: Real-time notifications via WebSockets/Kafka
- **External Oracles**: Price feeds and external data integration
- **IPFS Integration**: Decentralized document and media storage
- **Cross-chain Bridges**: Integration with other blockchain networks

### 5. Analytics & Reporting
- **Asset Analytics Dashboard**: Usage patterns and transfer statistics
- **Performance Metrics**: Chaincode operation monitoring
- **Custom Query Engine**: Advanced search and filtering capabilities
- **Data Export**: CSV/JSON export for external analysis
- **Compliance Reporting**: Regulatory compliance tools
- **ML-based Insights**: Predictive analytics for asset values

### 6. Scalability & Performance
- **Pagination Support**: Handle large datasets efficiently
- **Caching Layer**: Redis integration for frequently accessed data
- **Indexing Strategy**: Optimized database indexes
- **Horizontal Scaling**: Multi-peer deployment strategies
- **Data Archival**: Move old data to long-term storage

### 7. Implementation Examples

#### Enhanced Asset Structure
```go
type EnhancedAsset struct {
    ID             string            `json:"ID"`
    Category       AssetCategory     `json:"Category"`
    SubCategory    string            `json:"SubCategory"`
    Name           string            `json:"Name"`
    Description    string            `json:"Description"`
    Color          string            `json:"Color"`
    Size           int               `json:"Size"`
    Weight         float64           `json:"Weight"`
    Owner          string            `json:"Owner"`
    PreviousOwners []string          `json:"PreviousOwners"`
    AppraisedValue int               `json:"AppraisedValue"`
    MarketValue    int               `json:"MarketValue"`
    Metadata       map[string]string `json:"Metadata"`
    Tags           []string          `json:"Tags"`
    Status         AssetStatus       `json:"Status"`
    Location       Location          `json:"Location"`
    CreatedAt      time.Time         `json:"CreatedAt"`
    UpdatedAt      time.Time         `json:"UpdatedAt"`
    History        []AssetEvent      `json:"History"`
    Permissions    AssetPermissions  `json:"Permissions"`
}

type AssetCategory string
const (
    CategoryArt        AssetCategory = "ART"
    CategoryRealEstate AssetCategory = "REAL_ESTATE"
    CategoryVehicle    AssetCategory = "VEHICLE"
    CategoryJewelry    AssetCategory = "JEWELRY"
    CategoryCollectible AssetCategory = "COLLECTIBLE"
)

type AssetStatus string
const (
    StatusActive    AssetStatus = "ACTIVE"
    StatusSold      AssetStatus = "SOLD"
    StatusLost      AssetStatus = "LOST"
    StatusDestroyed AssetStatus = "DESTROYED"
    StatusEscrow    AssetStatus = "ESCROW"
)

type Location struct {
    Country  string  `json:"Country"`
    State    string  `json:"State"`
    City     string  `json:"City"`
    Address  string  `json:"Address"`
    Latitude float64 `json:"Latitude"`
    Longitude float64 `json:"Longitude"`
}

type AssetEvent struct {
    EventType   string    `json:"EventType"`
    FromOwner   string    `json:"FromOwner"`
    ToOwner     string    `json:"ToOwner"`
    Timestamp   time.Time `json:"Timestamp"`
    Transaction string    `json:"Transaction"`
    Value       int       `json:"Value"`
    Notes       string    `json:"Notes"`
}

type AssetPermissions struct {
    CanView     []string `json:"CanView"`
    CanEdit     []string `json:"CanEdit"`
    CanTransfer []string `json:"CanTransfer"`
    CanDelete   []string `json:"CanDelete"`
}
```

#### Advanced Query Functions
```go
// Query assets by owner with pagination
func (s *SmartContract) QueryAssetsByOwner(ctx contractapi.TransactionContextInterface, 
    owner string, pageSize int, bookmark string) (*AssetQueryResult, error) {
    
    queryString := fmt.Sprintf(`{
        "selector": {
            "Owner": "%s",
            "Status": "ACTIVE"
        },
        "sort": [{"CreatedAt": "desc"}]
    }`, owner)
    
    resultsIterator, responseMetadata, err := ctx.GetStub().GetQueryResultWithPagination(
        queryString, int32(pageSize), bookmark)
    if err != nil {
        return nil, err
    }
    defer resultsIterator.Close()
    
    assets := []*Asset{}
    for resultsIterator.HasNext() {
        queryResponse, err := resultsIterator.Next()
        if err != nil {
            return nil, err
        }
        
        var asset Asset
        err = json.Unmarshal(queryResponse.Value, &asset)
        if err != nil {
            return nil, err
        }
        assets = append(assets, &asset)
    }
    
    return &AssetQueryResult{
        Assets:   assets,
        Bookmark: responseMetadata.GetBookmark(),
        Count:    len(assets),
    }, nil
}

// Query assets by value range
func (s *SmartContract) QueryAssetsByValueRange(ctx contractapi.TransactionContextInterface, 
    minValue, maxValue int) ([]*Asset, error) {
    
    queryString := fmt.Sprintf(`{
        "selector": {
            "AppraisedValue": {
                "$gte": %d,
                "$lte": %d
            }
        }
    }`, minValue, maxValue)
    
    return s.getQueryResultForQueryString(ctx, queryString)
}

// Rich query with multiple filters
func (s *SmartContract) QueryAssetsWithFilters(ctx contractapi.TransactionContextInterface, 
    filters AssetFilters) ([]*Asset, error) {
    
    selector := map[string]interface{}{}
    
    if filters.Category != "" {
        selector["Category"] = filters.Category
    }
    if filters.Owner != "" {
        selector["Owner"] = filters.Owner
    }
    if filters.MinValue > 0 {
        selector["AppraisedValue"] = map[string]interface{}{
            "$gte": filters.MinValue,
        }
    }
    if len(filters.Tags) > 0 {
        selector["Tags"] = map[string]interface{}{
            "$in": filters.Tags,
        }
    }
    
    query := map[string]interface{}{
        "selector": selector,
        "sort": []map[string]string{{"UpdatedAt": "desc"}},
    }
    
    queryBytes, err := json.Marshal(query)
    if err != nil {
        return nil, err
    }
    
    return s.getQueryResultForQueryString(ctx, string(queryBytes))
}
```

#### Event Emission and Monitoring
```go
func (s *SmartContract) CreateAssetWithEvents(ctx contractapi.TransactionContextInterface, 
    asset Asset) error {
    
    // Validate asset
    if err := s.validateAsset(asset); err != nil {
        return err
    }
    
    // Create asset
    assetJSON, err := json.Marshal(asset)
    if err != nil {
        return err
    }
    
    err = ctx.GetStub().PutState(asset.ID, assetJSON)
    if err != nil {
        return err
    }
    
    // Emit creation event
    event := AssetEvent{
        EventType: "ASSET_CREATED",
        ToOwner:   asset.Owner,
        Timestamp: time.Now(),
        Value:     asset.AppraisedValue,
    }
    
    eventJSON, err := json.Marshal(event)
    if err != nil {
        return err
    }
    
    err = ctx.GetStub().SetEvent("AssetCreated", eventJSON)
    if err != nil {
        return fmt.Errorf("failed to set event: %v", err)
    }
    
    return nil
}
```

#### Multi-signature Transfer Function
```go
func (s *SmartContract) InitiateMultiSigTransfer(ctx contractapi.TransactionContextInterface, 
    assetID, newOwner string, requiredSignatures int) (string, error) {
    
    // Get current asset
    asset, err := s.ReadAsset(ctx, assetID)
    if err != nil {
        return "", err
    }
    
    // Create transfer proposal
    transferID := generateTransferID()
    proposal := TransferProposal{
        ID:                 transferID,
        AssetID:           assetID,
        CurrentOwner:      asset.Owner,
        ProposedOwner:     newOwner,
        RequiredSignatures: requiredSignatures,
        Signatures:        []string{},
        Status:            "PENDING",
        CreatedAt:         time.Now(),
        ExpiresAt:         time.Now().Add(24 * time.Hour),
    }
    
    proposalJSON, err := json.Marshal(proposal)
    if err != nil {
        return "", err
    }
    
    err = ctx.GetStub().PutState("TRANSFER_"+transferID, proposalJSON)
    if err != nil {
        return "", err
    }
    
    return transferID, nil
}

func (s *SmartContract) SignTransfer(ctx contractapi.TransactionContextInterface, 
    transferID string) error {
    
    // Get signer identity
    signerID, err := s.getSignerIdentity(ctx)
    if err != nil {
        return err
    }
    
    // Get transfer proposal
    proposalJSON, err := ctx.GetStub().GetState("TRANSFER_" + transferID)
    if err != nil {
        return err
    }
    
    var proposal TransferProposal
    err = json.Unmarshal(proposalJSON, &proposal)
    if err != nil {
        return err
    }
    
    // Add signature
    proposal.Signatures = append(proposal.Signatures, signerID)
    
    // Check if enough signatures
    if len(proposal.Signatures) >= proposal.RequiredSignatures {
        // Execute transfer
        err = s.executeTransfer(ctx, proposal)
        if err != nil {
            return err
        }
        proposal.Status = "EXECUTED"
    }
    
    // Update proposal
    proposalJSON, err = json.Marshal(proposal)
    if err != nil {
        return err
    }
    
    return ctx.GetStub().PutState("TRANSFER_"+transferID, proposalJSON)
}
```

## 📄 License

This project is licensed under the Apache License 2.0.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📞 Support

- **Documentation**: [Hyperledger Fabric Documentation](https://hyperledger-fabric.readthedocs.io/)
- **Community**: [Hyperledger Discord](https://discord.gg/hyperledger)

---

*Built with ❤️ for the Hyperledger Fabric community*