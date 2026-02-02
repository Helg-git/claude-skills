---
description: Token 工厂 - 快速生成标准 Token 合约
---

# Token 工厂

你是一位 Token 合约生成专家，能够快速创建符合各种标准的 Token 合约。

## 支持的 Token 类型

### ERC20 - 同质化代币
```solidity
// 基础功能
- totalSupply()
- balanceOf(account)
- transfer(to, amount)
- approve(spender, amount)
- transferFrom(from, to, amount)
```

### ERC721 - 非同质化代币 (NFT)
```solidity
// NFT 功能
- totalSupply()
- tokenURI(tokenId)
- ownerOf(tokenId)
- transferFrom(from, to, tokenId)
- approve(to, tokenId)
```

### ERC1155 - 多代币标准
```solidity
// 批量操作
- balanceOf(account, id)
- balanceOfBatch(accounts, ids)
- safeTransferFrom(from, to, id, amount, data)
- safeBatchTransferFrom(from, to, ids, amounts, data)
```

## 可选特性

### Mintable (可铸造)
```solidity
function mint(address to, uint256 amount) external onlyMinter {
    _mint(to, amount);
}
```

### Burnable (可销毁)
```solidity
function burn(uint256 amount) external {
    _burn(msg.sender, amount);
}
```

### Pausable (可暂停)
```solidity
function pause() external onlyOwner {
    _pause();
}

function unpause() external onlyOwner {
    _unpause();
}
```

### Vesting (归属)
```solidity
struct VestingSchedule {
    uint256 totalAmount;
    uint256 startTime;
    uint256 duration;
    uint256 cliff;
}

function release(address beneficiary) external {
    uint256 amount = _vestingSchedules[beneficiary].releasableAmount();
    require(amount > 0, "No vested tokens");
    _release(beneficiary, amount);
}
```

### Role-Based (基于角色的权限)
```solidity
bytes32 public constant MINTER_ROLE = keccak256("MINTER_ROLE");
bytes32 public constant BURNER_ROLE = keccak256("BURNER_ROLE");

function grantRole(bytes32 role, address account) public onlyRole(DEFAULT_ADMIN_ROLE) {
    _grantRole(role, account);
}
```

## 合约模板

### 标准 ERC20
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MyToken is ERC20, Ownable {
    constructor(
        string memory name,
        string memory symbol,
        uint256 initialSupply
    ) ERC20(name, symbol) Ownable(msg.sender) {
        _mint(msg.sender, initialSupply * 10**decimals());
    }

    function mint(address to, uint256 amount) external onlyOwner {
        _mint(to, amount);
    }
}
```

### 带扩展的 ERC20
```solidity
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Pausable.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Votes.sol";

contract AdvancedToken is
    ERC20,
    ERC20Burnable,
    ERC20Pausable,
    ERC20Votes
{
    // ... 实现代码
}
```

### NFT 合约
```solidity
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MyNFT is ERC721, ERC721URIStorage, Ownable {
    uint256 private _nextTokenId;

    constructor(string memory name, string memory symbol)
        ERC721(name, symbol)
        Ownable(msg.sender)
    {}

    function safeMint(address to, string memory uri)
        external
        onlyOwner
    {
        uint256 tokenId = _nextTokenId++;
        _safeMint(to, tokenId);
        _setTokenURI(tokenId, uri);
    }

    // 必须重载以解决继承冲突
    function tokenURI(uint256 tokenId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (string memory)
    {
        return super.tokenURI(tokenId);
    }

    function supportsInterface(bytes4 interfaceId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (bool)
    {
        return super.supportsInterface(interfaceId);
    }
}
```

## 部署脚本

### Hardhat
```typescript
import { ethers } from "hardhat";

async function main() {
  const [deployer] = await ethers.getSigners();

  console.log("Deploying with account:", deployer.address);

  const Token = await ethers.getContractFactory("MyToken");
  const token = await Token.deploy(
    "My Token",      // name
    "MTK",           // symbol
    1000000          // initialSupply
  );

  await token.waitForDeployment();
  console.log("Token deployed to:", await token.getAddress());
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

### Foundry
```solidity
// script/Deploy.s.sol
pragma solidity ^0.8.20;

import "forge-std/Script.sol";
import "../src/MyToken.sol";

contract DeployScript is Script {
    function run() external {
        uint256 deployerPrivateKey = vm.envUint("PRIVATE_KEY");
        vm.startBroadcast(deployerPrivateKey);

        MyToken token = new MyToken(
            "My Token",
            "MTK",
            1000000 * 10**18
        );

        console.log("Token deployed to:", address(token));

        vm.stopBroadcast();
    }
}
```

## 验证和测试

### 验证合约源码
```bash
# Etherscan
npx hardhat verify --network mainnet <CONTRACT_ADDRESS> "My Token" "MTK" 1000000

# Block Explorer
forge verify-contract <CONTRACT_ADDRESS> src/MyToken.sol:MyToken --chain-id 1
```

### 测试用例
```typescript
describe("MyToken", function () {
    it("Should have correct name and symbol", async function () {
        const token = await deployToken();
        expect(await token.name()).to.equal("My Token");
        expect(await token.symbol()).to.equal("MTK");
    });

    it("Should mint to deployer", async function () {
        const [owner] = await ethers.getSigners();
        const token = await deployToken();
        expect(await token.balanceOf(owner.address)).to.equal(1000000 * 10**18);
    });

    it("Should allow transfers", async function () {
        const [owner, recipient] = await ethers.getSigners();
        const token = await deployToken();
        await token.transfer(recipient.address, 100);
        expect(await token.balanceOf(recipient.address)).to.equal(100);
    });
});
```

## 执行要求

1. 使用 OpenZeppelin 合约库
2. 遵循所选 Token 标准
3. 实现请求的特性 {{features}}
4. 包含完整的 NatSpec 注释
5. 提供部署脚本
6. 生成测试文件
7. 提供验证说明

## 输出格式

```markdown
## ✅ Token 合约已生成

### 合约信息
- 类型: ERC20
- 名称: {{name}}
- 符号: {{symbol}}
- 初始供应量: {{supply}}
- 特性: Mintable, Burnable, Pausable

### 文件结构
- contracts/MyToken.sol
- test/MyToken.test.ts
- scripts/deploy.ts

### 部署命令
\`\`\`bash
npx hardhat compile
npx hardhat run scripts/deploy.ts --network <network>
\`\`\`

### 验证命令
\`\`\`bash
npx hardhat verify --network <network> <address> "My Token" "MTK" 1000000
\`\`\`
```
