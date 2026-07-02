// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract BaseToken is ERC20, Ownable {
    uint256 public constant MAX_SUPPLY = 1_000_000 * 10 ** 18; // 1M tokens

    event Minted(address indexed to, uint256 amount);

    constructor() ERC20("BaseToken", "BASE") Ownable(msg.sender) {
        // Mint initial supply to deployer
        _mint(msg.sender, 100_000 * 10 ** decimals());
    }

    function mint(address to, uint256 amount) external onlyOwner {
        require(totalSupply() + amount <= MAX_SUPPLY, "Cap exceeded");
        _mint(to, amount);
        emit Minted(to, amount);
    }
}
