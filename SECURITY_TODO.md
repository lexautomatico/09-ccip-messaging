# Security To-Do for CCIP Messaging

## Environment Variables Setup

This file outlines the environment variables that need to be set up for secure deployment of the CCIP Messaging application.

### Required Environment Variables

The following environment variables should be configured:

#### Critical Security Variables
- **PRIVATE_KEY**: Ethereum private key for contract deployments and transactions
  - **CRITICAL**: Private keys provide full control of wallets and funds - highest security priority

#### API Keys
- **INFURA_KEY**: API key for Infura blockchain access
- **ALCHEMY_API_KEY**: API key for Alchemy blockchain services

#### Blockchain Explorer API Keys
- **ETHERSCAN_API_KEY**: API key for Etherscan verification
- **ARBISCAN_API_KEY**: API key for Arbiscan verification
- **OPTIMISM_API_KEY**: API key for Optimism Explorer verification
- **POLYGONSCAN_API_KEY**: API key for Polygonscan verification
- **BASESCAN_API_KEY**: API key for Basescan verification
- **LINEASCAN_API_KEY**: API key for Lineascan verification

#### RPC URLs
- **ETHEREUM_RPC_URL**: Ethereum RPC endpoint
- **ARBITRUM_RPC_URL**: Arbitrum RPC endpoint
- **OPTIMISM_RPC_URL**: Optimism RPC endpoint
- **POLYGON_RPC_URL**: Polygon RPC endpoint
- **BASE_RPC_URL**: Base RPC endpoint
- **LINEA_RPC_URL**: Linea RPC endpoint

### Implementation Steps

1. Create a `.env` file in the project root with the following template (replace placeholders with actual values):
   ```
   # CRITICAL: NEVER COMMIT THIS FILE TO VERSION CONTROL
   
   # Private key - HIGHEST SECURITY PRIORITY
   PRIVATE_KEY=your_new_private_key_here
   
   # API Keys
   INFURA_KEY=your_infura_api_key_here
   ALCHEMY_API_KEY=your_alchemy_api_key_here
   
   # Explorer API Keys
   ETHERSCAN_API_KEY=your_etherscan_api_key_here
   ARBISCAN_API_KEY=your_arbiscan_api_key_here
   OPTIMISM_API_KEY=your_optimism_explorer_api_key_here
   POLYGONSCAN_API_KEY=your_polygonscan_api_key_here
   BASESCAN_API_KEY=your_basescan_api_key_here
   LINEASCAN_API_KEY=your_lineascan_api_key_here
   
   # RPC URLs - Can be constructed with API keys or use dedicated endpoints
   ETHEREUM_RPC_URL=https://mainnet.infura.io/v3/your_infura_key_here
   ARBITRUM_RPC_URL=https://arbitrum-mainnet.infura.io/v3/your_infura_key_here
   OPTIMISM_RPC_URL=https://optimism-mainnet.infura.io/v3/your_infura_key_here
   POLYGON_RPC_URL=https://polygon-mainnet.infura.io/v3/your_infura_key_here
   BASE_RPC_URL=https://base-mainnet.infura.io/v3/your_infura_key_here
   LINEA_RPC_URL=https://linea-mainnet.infura.io/v3/your_infura_key_here
   ```

2. Update `hardhat.config.js` to ensure all networks use environment variables:
   ```javascript
   // Replace hardcoded keys with environment variables
   module.exports = {
     networks: {
       ethereum: {
         url: process.env.ETHEREUM_RPC_URL,
         accounts: [process.env.PRIVATE_KEY]
       },
       arbitrum: {
         url: process.env.ARBITRUM_RPC_URL,
         accounts: [process.env.PRIVATE_KEY]
       },
       // ... other networks
     },
     etherscan: {
       apiKey: {
         mainnet: process.env.ETHERSCAN_API_KEY,
         polygon: process.env.POLYGONSCAN_API_KEY,
         arbitrumOne: process.env.ARBISCAN_API_KEY,
         // ... other networks
       }
     }
   };
   ```

3. Update frontend configuration:
   - In `scripts/update-frontend-config.js`, replace hardcoded Infura API key with environment variables
   - In `public/js/config.js`, ensure all RPC URLs use environment variables

4. Add the `.env` file to `.gitignore` to prevent committing secrets:
   ```
   # .gitignore
   .env
   .env.local
   .env.development
   .env.production
   ```

5. For production deployment, set these environment variables in your hosting platform (Vercel, etc.)

### Additional Steps for This Project

1. **URGENT - Rotate All Compromised Credentials**:
   - Generate a new private key and transfer any assets from the old address
   - Rotate all API keys that have been exposed
   - Update `.env` files and deployment environments with new keys

2. **Private Key Security**:
   - Consider using a hardware wallet or secure key management solution instead of storing private keys in `.env`
   - For local development, use a dedicated test account with minimal funds
   - Use different wallets for development/testing vs. production

3. **Update CCIP Router Configuration**:
   - Review any contracts using these credentials
   - Ensure contract ownership is properly secured
   - Update any deployed contracts that reference old credentials

4. **Implement Access Controls**:
   - Set up rate limiting for API calls
   - Implement proper role-based access controls
   - Add validation to fail safely when credentials are missing

### Security Best Practices

- **CRITICAL**: Private keys should NEVER be committed to version control
- Create a wallet specifically for development with minimal funds
- Rotate API keys periodically and after any suspected exposure
- Use different API keys for development and production
- Implement proper access controls for all services
- Consider using a secrets management service for production
- Regularly audit code for hardcoded credentials
- Educate team members about secure credential management practices